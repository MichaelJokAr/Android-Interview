> **说明**：部分内容来自网络，若有侵权请联系我删除

## Android - [Android基础](./README.md) - [Java](./Java.md) - [网络](./network.md)



## **Q：handler 发送延迟消息是怎么实现的**
- postDelay()一个10秒钟的Runnable A、消息进队，MessageQueue调用nativePollOnce()阻塞，Looper阻塞；
- 紧接着post()一个Runnable B、消息进队，判断现在A时间还没到、正在阻塞，把B插入消息队列的头部（A的前面），然后调用nativeWake()方法唤醒线程；
- MessageQueue.next()方法被唤醒后，重新开始读取消息链表，第一个消息B无延时，直接返回给Looper；
- Looper处理完这个消息再次调用next()方法，MessageQueue继续读取消息链表，第二个消息A还没到时间，计算一下剩余时间（假如还剩9秒）继续调用nativePollOnce()阻塞；直到阻塞时间到或者下一次有Message进队；

[https://www.jianshu.com/p/1b475dc531b1](https://www.jianshu.com/p/1b475dc531b1)

## **Q：Handler同步屏障**

同步屏障机制就是插入一个同步屏障信息到Looper的队列头部，当Looper调用next获取信息时候，发现message的target为null时开始处理同步屏障信息，就会跳过所有同步消息，寻找为异步的消息返回给looper

[https://www.wanandroid.com/wenda/show/8710](https://www.wanandroid.com/wenda/show/8710)

## **Q：Message的缓存**
[https://www.cnblogs.com/leipDao/p/7850473.html](https://www.cnblogs.com/leipDao/p/7850473.html)

## **Q: handldr实现线程通信**

[出处](https://www.cnblogs.com/angrycode/p/6576905.html)

当创建```Handler```时将通过```ThreadLocal```在当前线程绑定一个``Looper``对象，而```Looper```持有```MessageQueue```对象。执行```Handler.sendMessage(Message)```方法将一个待处理的```Message```插入到```MessageQueue```中，这时候通过```Looper.loop()```方法获取到队列中```Message```，然后再交由```Handler.handleMessage(Message)```来处理。

## **Q: Handler中有loop死循环，为什么没有阻塞主线程？原理是什么**

[出处](https://www.jianshu.com/p/ea7beaeeee16)
### 相关
- 对于线程即是一段可执行的代码，当可执行代码执行完成后，线程生命周期便该终止了，线程退出。
- 而对于主线程，我们是绝不希望会被运行一段时间，自己就退出，那么如何保证能一直存活呢？简单做法就是可执行代码是能一直执行下去的，死循环便能保证不会被退出，例如，binder线程也是采用死循环的方法，通过循环方式不同与Binder驱动进行读写操作，当然并非简单地死循环，无消息时会休眠。
- 但这里可能又引发了另一个问题，既然是死循环又如何去处理其他事务呢？
<br>--> 通过创建新线程的方式。

- 真正会卡死主线程的操作是在回调方法onCreate/onStart/onResume等操作时间过长，会导致掉帧，甚至发生ANR，looper.loop本身不会导致应用卡死。

## **Q：主线程的死循环一直运行是不是特别消耗CPU资源呢？**

 其实不然，这里就涉及到Linux pipe/epoll机制，简单说就是在主线程的MessageQueue没有消息时，便阻塞在loop的queue.next()中的nativePollOnce()方法里，此时主线程会释放CPU资源进入休眠状态，直到下个消息到达或者有事务发生，通过往pipe管道写端写入数据来唤醒主线程工作。这里采用的epoll机制，是一种IO多路复用机制，可以同时监控多个描述符，当某个描述符就绪(读或写就绪)，则立刻通知相应程序进行读或写操作，本质同步I/O，即读写是阻塞的。
 <br> 所以说，主线程大多数时候都是处于休眠状态，并不会消耗大量CPU资源。 Gityuan–Handler(Native层)

## **Q：Handler 是如何能够线程切换**

**Handler创建的时候会采用当前线程的Looper来构造消息循环系统，Looper在哪个线程创建，就跟哪个线程绑定，并且Handler是在他关联的Looper对应的线程中处理消息的。**

那么Handler 是如何能够线程切换？是通过```ThreadLocal``` ! ```ThreadLocal```可以在不同的线程中互不干扰的存储并提供数据，通过```ThreadLocal```可以轻松获取每个线程的``Looper``。

## **Q：ThreadLocal的工作原理**

 > “```ThreadLocal``` 是一个线程内部的数据存储类，通过它可以在指定的线程中存储数据，数据存储以后只有在指定线程中可以获取到存储的数据。对于其他线程来说则无法获取到数据” ---<Android开发艺术探索>

[链接](https://blog.csdn.net/singwhatiwanna/article/details/48350919)

 从ThreadLocal的set和get方法可以看出，它们所操作的对象都是当前线程的localValues对象的table数组，因此在不同线程中访问同一个ThreadLocal的set和get方法，它们对ThreadLocal所做的读写操作仅限于各自线程的内部，这就是为什么ThreadLocal可以在多个线程中互不干扰地存储和修改数据，理解ThreadLocal的实现方式有助于理解Looper的工作原理。

## **Q: ThreadLocal底层原理和Handler关系**

[https://www.jianshu.com/p/acef5aa8d2c6/](https://www.jianshu.com/p/acef5aa8d2c6/)

## **Q：looper quit和quitSafely区别**
quit和quitSafely均会调用MessageQueue.java的quit方法，只不过传值不同罢了；当调用quit的时候，其间接调用的是removeAllMessagesLocked方法，而quitSafely其间接调用的是removeAllFutureMessagesLocked方法；

```java
            if (safe) {
                removeAllFutureMessagesLocked();
            } else {
                removeAllMessagesLocked();
            }
```
```java
    private void removeAllMessagesLocked() {
        Message p = mMessages;
        while (p != null) {
            Message n = p.next;
            p.recycleUnchecked();
            p = n;
        }
        mMessages = null;
    }
```

```java
    private void removeAllFutureMessagesLocked() {
        final long now = SystemClock.uptimeMillis();
        Message p = mMessages;
        if (p != null) {
            if (p.when > now) {
                removeAllMessagesLocked();
            } else {
                Message n;
                for (;;) {
                    n = p.next;
                    if (n == null) {
                        return;
                    }
                    if (n.when > now) {
                        break;
                    }
                    p = n;
                }
                p.next = null;
                do {
                    p = n;
                    n = p.next;
                    p.recycleUnchecked();
                } while (n != null);
            }
        }
    }
```

## **Q：onmesure 两个参数**

它有三种模式：
- UNSPECIFIED（未指定）：父元素不对子元素施加任何束缚，子元素可以得到任意想要的大小；
- EXACTLY（完全）：父元素决定子元素的确切大小，子元素将被限定在给定的边界里而忽略它本身的大小；
- AT_MOST（最多）：子元素至最多达到指定大小的值。

```java
switch (specMode) {  
// Parent has imposed an exact size on us  
//1、父View是EXACTLY的 ！  
case MeasureSpec.EXACTLY:   
    //1.1、子View的width或height是个精确值 (an exactly size)  
    if (childDimension >= 0) {            
        resultSize = childDimension;         //size为精确值  
        resultMode = MeasureSpec.EXACTLY;    //mode为 EXACTLY 。  
    }   
    //1.2、子View的width或height为 MATCH_PARENT/FILL_PARENT   
    else if (childDimension == LayoutParams.MATCH_PARENT) {  
        // Child wants to be our size. So be it.  
        resultSize = size;                   //size为父视图大小  
        resultMode = MeasureSpec.EXACTLY;    //mode为 EXACTLY 。  
    }   
    //1.3、子View的width或height为 WRAP_CONTENT  
    else if (childDimension == LayoutParams.WRAP_CONTENT) {  
        // Child wants to determine its own size. It can't be  
        // bigger than us.  
        resultSize = size;                   //size为父视图大小  
        resultMode = MeasureSpec.AT_MOST;    //mode为AT_MOST 。  
    }  
    break;  

// Parent has imposed a maximum size on us  
//2、父View是AT_MOST的 ！      
case MeasureSpec.AT_MOST:  
    //2.1、子View的width或height是个精确值 (an exactly size)  
    if (childDimension >= 0) {  
        // Child wants a specific size... so be it  
        resultSize = childDimension;        //size为精确值  
        resultMode = MeasureSpec.EXACTLY;   //mode为 EXACTLY 。  
    }  
    //2.2、子View的width或height为 MATCH_PARENT/FILL_PARENT  
    else if (childDimension == LayoutParams.MATCH_PARENT) {  
        // Child wants to be our size, but our size is not fixed.  
        // Constrain child to not be bigger than us.  
        resultSize = size;                  //size为父视图大小  
        resultMode = MeasureSpec.AT_MOST;   //mode为AT_MOST  
    }  
    //2.3、子View的width或height为 WRAP_CONTENT  
    else if (childDimension == LayoutParams.WRAP_CONTENT) {  
        // Child wants to determine its own size. It can't be  
        // bigger than us.  
        resultSize = size;                  //size为父视图大小  
        resultMode = MeasureSpec.AT_MOST;   //mode为AT_MOST  
    }  
    break;  

// Parent asked to see how big we want to be  
//3、父View是UNSPECIFIED的 ！  
case MeasureSpec.UNSPECIFIED:  
    //3.1、子View的width或height是个精确值 (an exactly size)  
    if (childDimension >= 0) {  
        // Child wants a specific size... let him have it  
        resultSize = childDimension;        //size为精确值  
        resultMode = MeasureSpec.EXACTLY;   //mode为 EXACTLY  
    }  
    //3.2、子View的width或height为 MATCH_PARENT/FILL_PARENT  
    else if (childDimension == LayoutParams.MATCH_PARENT) {  
        // Child wants to be our size... find out how big it should  
        // be  
        resultSize = 0;                        //size为0！ ,其值未定  
        resultMode = MeasureSpec.UNSPECIFIED;  //mode为 UNSPECIFIED  
    }   
    //3.3、子View的width或height为 WRAP_CONTENT  
    else if (childDimension == LayoutParams.WRAP_CONTENT) {  
        // Child wants to determine its own size.... find out how  
        // big it should be  
        resultSize = 0;                        //size为0! ，其值未定  
        resultMode = MeasureSpec.UNSPECIFIED;  //mode为 UNSPECIFIED  
    }  
    break;  
}  
```
[https://blog.csdn.net/jdsjlzx/article/details/53428288](https://blog.csdn.net/jdsjlzx/article/details/53428288)

[https://segmentfault.com/a/1190000007948959](https://segmentfault.com/a/1190000007948959)

## **Q：retrofit 源码解析**

[https://www.jianshu.com/p/0c055ad46b6c](https://www.jianshu.com/p/0c055ad46b6c)

## **Q：android gc 是什么时候发生的**

- GC_FOR_MALLOC: 表示是在堆上分配对象时内存不足触发的GC。
- GC_CONCURRENT: 当我们应用程序的堆内存达到一定量，或者可以理解为快要满的时候，系统会自动触发GC操作来释放内存。
- GC_EXPLICIT: 表示是应用程序调用System.gc、VMRuntime.gc接口或者收到SIGUSR1信号时触发的GC。
- GC_BEFORE_OOM: 表示是在准备抛OOM异常之前进行的最后努力而触发的GC。

[https://www.cnblogs.com/purpleraintear/p/6046441.html](https://www.cnblogs.com/purpleraintear/p/6046441.html)

## **Q：背景透明会影响过度绘制吗**
不会影响

## **Q：lruCache 原理**
[https://www.jianshu.com/p/b49a111147ee](https://www.jianshu.com/p/b49a111147ee)


## **Q：怎么性能优化的**
[https://www.cnblogs.com/xgjblog/p/4089594.html](https://www.cnblogs.com/xgjblog/p/4089594.html)

## **Q：Android  自定义view**


## **Q：setContentView 过程**


[https://www.cnblogs.com/leipDao/p/7509222.html](https://www.cnblogs.com/leipDao/p/7509222.html)

## **Q：Activity视图层级**

![](https://img-blog.csdn.net/20170720120635514?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvWlhDNjQxNDgzNTcz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
```
phoneWindow (层一)
    -> DecorView （层二）
            -> contentView （层三）
                -> 自己的布局 (层四)
    -> 系统布局（TitleBar、StatusBar）（层二）
```

[https://blog.csdn.net/zxc641483573/article/details/75508258](https://blog.csdn.net/zxc641483573/article/details/75508258)

###  toolbar navigationBar 跟activiy一个层级吗？或者他们在哪个层级

不在，在 DecorView （层二） 层级里



## **Q:事件传递**

![](https://upload-images.jianshu.io/upload_images/2001124-111a6c68da9d4df1.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)


### action_cancel什么时候执行
    在父View中拦截ACTION_UP或ACTION_MOVE，在第一次父视图拦截消息的瞬间，父视图指定子视图不接受后续消息了，同时子视图会收到ACTION_CANCEL事件。

## **Q: Android垃圾回收机制**

[出处](https://blog.csdn.net/u012505618/article/details/78415275)

常见的垃圾回收算法有引用计数法（Reference Counting）、标注并清理（Mark and Sweep GC）、拷贝（Copying GC）和逐代回收（Generational GC）等算法，其**中Android系统采用的是标注并删除和拷贝GC**，并不是大多数JVM实现里采用的逐代回收算法

### 标注并清理回收法（Mark and Sweep GC）

在这个算法中，程序在运行的过程中不停的创建新的对象并消耗内存，直到内存用光，这时再要创建新对象时，系统暂停其它组件的运行，触发GC线程启动垃圾回收过程。内存回收的原理很简单，就是从所谓的"GC Roots"集合开始，将内存整个遍历一次，保留所有可以被GC Roots直接或间接引用到的对象，而剩下的对象都当作垃圾对待并回收，如代码清单14 - 2：
<center>代码清单14 - 2 标注并清理算法伪码</center>  

```
void GC(){
   SuspendAllThreads();

   List<Object> roots = GetRoots();

   foreach ( Object root : roots ) {

     Mark(root);

   }

   Sweep();

   ResumeAllThreads();

}
```

算法通常分为两个主要的步骤：
- 标注（Mark）阶段：这个过程的伪码如代码清单14 - 2所示，针对GC Roots中的每一个对象，采用递归调用的方式（第8行）处理其直接和间接引用到的所有对象：
<center>代码清单14 - 3 标注并清理的标注阶段伪码</center>

```
    void Mark(Object* pObj) {
    if ( !pObj->IsMarked() ) {
        // 修改对象头的Marked标志
        pObj->Mark();
    // 深度优先遍历对象引用到的所有对象
    List<Object*> fields = pObj->GetFields();
    foreach ( Object* field : fields ) {
        Make(field); // 递归处理引用到的对象
    }
    }
    }  
```

如果对象引用的层次过深，递归调用消耗完虚拟机内GC线程的栈空间，从而导致栈空间溢出（StackOverflow）异常，为了避免这种情况的发生，在具体实现时，通常是用一个叫做标注栈（Mark Stack）的数据结构来分解递归调用。一开始，标注栈（Mark Stack）的大小是固定的，但在一些极端情况下，如果标注栈的空间也不够的话，则会分配一个新的标注栈（Mark Stack），并将新老栈用链表连接起来。与引用计数法中对象的内存布局类似，对象是否被标注的标志也是保存在对象头里的，如图 14 - 2所示


![图 14 - 2 标注和清理算法中的对象布局](http://images.cnitblog.com/blog/49788/201306/12111950-792bcbda6b1b434395db2528f9074740.png) 

    如图 14 - 2是垃圾回收前的对象之间的引用关系；GC线程遍历完整个内存堆之后，标识出所以可以被"GC Roots"引用到的对象－即代码清单14 - 2中的第4行，结果如图 14 - 3中高亮的部分，对于所有未被引用到（即未被标注）的对象，都将其作为垃圾收集。
    
![图 14 - 3 回收内存垃圾之前的对象引用关系](http://images.cnitblog.com/blog/49788/201306/12111951-a62f397ce02443d3ad6c7d79d28f5a0a.png)

![图 14 - 4 GC线程标识出所有不能被回收的对象实例](http://images.cnitblog.com/blog/49788/201306/12111951-4184d3205adb4f7ea7e1abb83344cfd4.png)

<br>

- 清理（SWEEP）阶段：即执行垃圾回收过程，留下有用的对象，如图 14 - 4所示，代码清单14 - 3是这个过程的伪码，在这个阶段，GC线程遍历整个内存，将所有没有标注的对象（即垃圾）全部回收，并将保留下来的对象的标志清除掉，以便下次GC过程中使用。
<center>代码清单14 - 4 标注和清理法中的清理过程伪码</center>

```
void Sweep() {
Object *pIter = GetHeapBegin();
     while ( pIter < GetHeapEnd() ) {
     if ( !pIter->IsMarked() )
     Free(pIter);
     else
     pIter->UnMark();
 
     pIter = MoveNext(pIter);
  }
}

```


![图 14 - 5 GC线程执行完垃圾回收过程后的对象图](http://images.cnitblog.com/blog/49788/201306/12111951-a791b1ba05b04ed4a6dd6e8fd816f654.png)

这个方法的优点是很好地处理了引用计数中的循环引用问题，而且在内存足够的前提下，对程序几乎没有任何额外的性能开支（如不需要维护引用计数的代码等），然而它的一个很大的缺点就是在执行垃圾回收过程中，需要中断进程内其它组件的执行

### 拷贝回收法（Copying GC）
这也是标注法的一个变种， GC内存堆实际上分成乒（ping）和乓（pong）两部分。一开始，所有的内存分配请求都有乒（ping）部分满足，其维护"下个对象分配的起始位置"指针，分配内存仅仅就是操作下这个指针而已，当乒（ping）的内存快用完时，采用标注（Mark）算法识别出存活的对象，如图 14 - 9所示，并将它们拷贝到乓（pong）部分，后续的内存分配请求都在乓（pong）部分完成，如图 14 - 10。而乓（pong）里的内存用完后，再切换回乒（ping）部分，使用内存就跟打乒乓球一样。

![图 14 - 9 拷贝回收法中的乒乓内存块](http://images.cnitblog.com/blog/49788/201306/12111952-2441543b673242fe870b7d4ef37bd393.png)
![图 14 - 10 拷贝回收法中的切换乒乓内存块以满足内存分配请求](http://images.cnitblog.com/blog/49788/201306/12111952-68b161d6d30a479aad702181b907382a.png)

回收算法的优点在于内存分配速度快，而且还有可能实现低中断，因为在垃圾回收过程中，从一块内存拷贝存活对象到另一块内存的同时，还可以满足新的内存分配请求，但其缺点是需要有额外的一个内存空间。不过对于回收算法的缺点，也可以通过操作系统地虚拟内存提供的地址空间申请和提交分布操作的方式实现优化，因此在一些JVM实现中，其Eden区域内的垃圾回收采用此算法。



## **Q：子线程更新ui有哪些方法**


主线程中定义Handler，子线程通过mHandler发送消息，主线程Handler的handleMessage更新UI。 用Activity对象的runOnUiThread方法。 创建Handler，传入getMainLooper。 View.post(Runnable r) 。


## **Q: Glide对Bitmap的缓存与解码复用如何做到**


[链接](https://www.jianshu.com/p/00540c9a4de9)


## **Q：RecyclerView的ViewHolder复用**


[https://cloud.tencent.com/developer/article/1128137](https://cloud.tencent.com/developer/article/1128137)

[RecyclerView的ViewHolder复用机制总结](https://duanjobs.github.io/2017/10/18/RecyclerView%E7%9A%84ViewHolder%E5%A4%8D%E7%94%A8%E6%9C%BA%E5%88%B6%E6%80%BB%E7%BB%93/)

## **Q: 给你一个Demo你如何快速定位ANR**

contentProvider监听trace.txt文件或者用GitHub的ANGWatchDog 工具

## **Q: 对Dalvik虚拟机的认识**


[链接](https://www.jianshu.com/p/6bdbbab73705)

Android的虚拟机系统是Dalvik虚拟机，是Google等商家合作开发的Android移动设备平台的核心组件之一。DalvikVM可以支持已转为.dex格式的Java应用程序的运行，其中“.dex”格式是专为DVM设计的一种压缩格式，适合内存和处理器速度都有限的系统。现实中的大多数虚拟机都是一种基于堆栈的机器，例如JVM，而DVM则是基于寄存器的。基于栈的机器需要更多指令，而基于寄存器的机器指令更大。

## **Q: Android虚拟机Dalvik与ART区别**


ART 的机制与 Dalvik 不同。在Dalvik下，应用每次运行的时候，字节码都需要通过即时编译器（just in time ，JIT）转换为机器码，这会拖慢应用的运行效率，而在ART 环境中，应用在第一次安装的时候，字节码就会预先编译成机器码，使其成为真正的本地应用。这个过程叫做预编译（AOT,Ahead-Of-Time）。这样的话，应用的启动(首次)和执行都会变得更加快速。

## **Q: 进程保活如何做到，保活率有多高**


## **Q: Binder通信原理与机制**


![](https://upload-images.jianshu.io/upload_images/14140248-1cf40a3e489fcf9a.png)

[https://www.cnblogs.com/1157760522ch/p/11181982.html](https://www.cnblogs.com/1157760522ch/p/11181982.html)

## **Q: Linux自带多种进程通信方式，为什么android都没有采用而是用Binder通信**


- (1) 从性能的角度 数据拷贝次数：

  Binder数据拷贝只需要一次，而管道、消息队列、Socket都需要2次，但共享内存方式一次内存拷贝都不需要；从性能角度看，Binder性能仅次于共享内存。

- (2) 从稳定性的角度：

  Binder是基于C/S架构的，简单解释下C/S架构，是指客户端(Client)和服务端(Server)组成的架构，Client端有什么需求，直接发送给Server端去完成，架构清晰明朗，Server端与Client端相对独立，稳定性较好；而共享内存实现方式复杂，没有客户与服务端之别， 需要充分考虑到访问临界资源的并发同步问题，否则可能会出现死锁等问题；从这稳定性角度看，Binder架构优越于共享内存。

  仅仅从以上两点，各有优劣，还不足以支撑google去采用binder的IPC机制，那么更重要的原因是：

- (3) 从安全的角度：

  传统Linux的IPC无法 获得对方进程的PID\UID，从而无法鉴别对象的身份。Android作为开源的OS，App来源广泛，如果不加鉴别的允许一些非法软件运行在用户的手机上，这将是非常危险的。
Android为每一个安装好的App分配自己的的UID，所以进程的UID是鉴别身份的重要标志，Binder是基于C\S架构的，对外只暴漏Client端，Client端将请求发送给Server端，Server根据权限控制策略，来判断UID\PID是否满足访问权限。从Android M开始，系统会弹出对话框来询问用户是否要给与权限，安全性极大的提高，所以从安全性角度来看Binder是要优于其他传统的IPC的。

[https://blog.csdn.net/angji6738/article/details/102246055](https://blog.csdn.net/angji6738/article/details/102246055)

## **Q: Actiivty启动流程**


> Activity的启动过程，我们可以从Context的startActivity说起，其实现是ContextImpl的startActivity，然后内部会通过Instrumentation来尝试启动Activity，这是一个跨进程过程，它会调用ams的startActivity方法，当ams校验完activity的合法性后，会通过ApplicationThread回调到我们的进程，这也是一次跨进程过程，而applicationThread就是一个binder，回调逻辑是在binder线程池中完成的，所以需要通过Handler H将其切换到ui线程，第一个消息是LAUNCH_ACTIVITY，它对应handleLaunchActivity，在这个方法里完成了Activity的创建和启动，接着，在activity的onResume中，activity的内容将开始渲染到window上，然后开始绘制直到我们看见。-——任玉刚大佬


## **Q: onActivityResult能不能设计成回调？**

由于匿名内部类会持有外部类的引用，当Activity A被意外销毁时，由于其对象被OnResultCallback可能会导致其无法正常被GC，当Activity B给Activity A回复时，OnResultCallback被调用，新的Activity A2 被恢复，但是onResult方法持有的依旧是被销毁的Activity A的引用，即：

[https://blog.csdn.net/u014756589/article/details/100830044](https://blog.csdn.net/u014756589/article/details/100830044)

## **Q: AMS在Android中的作用是什么？Activity的启动跟AMS有什么关系**

>AMS是Android中最核心的服务，主要负责系统中四大组件的启动、切换、调度及应用进程的管理和调度等工作，其职责与操作系统中的进程管理和调度模块相类似，因此它在Android中非常重要。

## **Q: tinker热修复原理**


[https://juejin.im/post/5b640deef265da0f86544bb1](https://juejin.im/post/5b640deef265da0f86544bb1)

## **Q: PMS了解过吗？怎么看？聊聊PMS详细实现过程**

> PackageManagerService（简称 PKMS），是 Android 系统中核心服务之一，管理着所有跟 package 相关的工作，常见的比如安装、卸载应用。PackageManagerService 是在 SystemServer 进程中启动的.

## **Q: 热修复**


## **Q: 增量升级**


## **Q: 设计一个多用户，多角色多APP架构**



## **Q: butterknife为什么执行效率比其他框架效率高？原理是什么**

使用了编译时生成代码，比运行时发射效率更快

## **Q: Android压缩步骤**



## **Q: 如何彻底防止反编译，dex加密怎么做**


加固

## **Q: AOP与OOP都区别、原理**


## **Q: 序列花与反序列化的原理，android的parcelable与sericlizabl的区别**

- parcelable： 内存中使用效率更快，因为序列化时不会产生过多的零时变量导致频繁gc
- sericlizabl：本地持久化的时候更好
### 区别
- 1）设计目的
    - Serializable是Java API,是一个通用的序列化机制，通过将文件保存到本地文件、网络流等实现便数据的传递，这种数据传递不仅可以在单个程序中进行，也可以在两个不同的程序中进行；
    - Parcelable是Android SDK API,为了在同个程序的不同组件之间和不同程序（AIDL）之间高效的传输数据，是通过IBinder通信的消息的载体。从设计目的上可以看出Parcelable就是为了Android高效传输数据而生的。
- 2）实现原理
    - Serializable是通过I/O读写存储在磁盘上的,使用反射机制，序列化过程较慢，且在序列化过程中创建许多临时对象，容易触发GC。
    - Parcelable是直接在内存中读写的，自已实现封送和解封（marshalled &unmarshalled）操作，将一个完整的对象分解成Intent所支持的数据类型，不需要使用反射，所以Parcelable具有效率高，内存开销小的优点。

## **Q：Parcelable为了效率损失了什么**
Serializable是通用的序列化机制的，将数据存储在磁盘，可以做到有限持久化保存，文件的生命周期不受程序影响，Parcelable的序列化操作完全由底层实现，不同版本的Android是实现方式可能不相同，所以不能进行持久化存储。


## **Q: 手机qq换肤与原理**


## **Q: 如何实现直播效果**


## **Q: 抖音直播中网速较差情况下，如何使画面流畅**


## **Q: 音频同步原理、音频能绝对同步吗**


## **Q: 硬编码和软编码区别？录屏时如何选取硬编和软编**


## **Q: incloud、merge、viewStub的作用和原理**

- include: 提高include里面布局的复用，便于对相同视图内容进行统一的控制管理，提高布局重用性
- merge: 减少视图的节点数，从而减少视图在绘制过程消耗的时间，达到提高UI性能的效果。
- ViewSub: 需要时才加载绘制

## **Q: 为什么RecyclerView加载首屏会慢一些**


## **Q: view绘制机制，onMeasure、onDraw方法调用机制**


## **Q: 为什么android会出现卡顿**


## **Q: syync关键字和 lock区别？他们对线程控制原理**


## **Q: EventBus AOP面向切面编程原理**


## **Q: 饿了么Hermes跨进程架构原理**


## **Q: 阿里ARouter**


## **Q: Rxjava**


## **Q: UI绘制流程和原理**
[https://www.cnblogs.com/joahyau/p/11294970.html](https://www.cnblogs.com/joahyau/p/11294970.html)

## **onmeasure为什么会被调用两次**
- 当```ViewRootImpl.performTraversals```方法里，第一次```perforMeasuer```测量完后子view对父view给的测量结果不满意需要扩容，则会进行第二次```perforMeasuer```方法调用进行第二次```onMeasure```

## **Q：MVVM**
- Model：数据模型(实体类、持久化、IO)
- View：Activity/Fragment和布局文件
- ViewModel：业务逻辑的处理、数据的转换、连接M层和V层的桥梁


与MVP基本相同，最大的不同就是model更新后不需要再通过presenter更新view，mvvm可以自动更新



[https://blog.csdn.net/yulidrff/article/details/85330045](https://blog.csdn.net/yulidrff/article/details/85330045)

## **Q: 算法：hash值、HashMap、最小生成树算法、KMP算法、查找算法、排序算法**



## **子线程startActivity**
startActivity(Intent) 的底层实现是将 intent分解成任务，传递到mainLooper 轮询的队列中，最终由主线程执行。所以跟由哪个线程调用一点关系都没有的。

```
final boolean realStartActivityLocked(ActivityRecord r, ProcessRecord app,
            boolean andResume, boolean checkConfig) throws RemoteException  {
    // 省略代码
    app.thread.scheduleLaunchActivity(new Intent(r.intent), r.appToken,
                    System.identityHashCode(r), r.info, new Configuration(mService.mConfiguration),
                    new Configuration(task.mOverrideConfig), r.compat, r.launchedFromPackage,
                    task.voiceInteractor, app.repProcState, r.icicle, r.persistentState, results,
                    newIntents, !andResume, mService.isNextTransitionForward(), profilerInfo);
}  
```

> 参考链接
    
- [Android 7.0 startActivity()源码解析](https://www.jianshu.com/p/86ad1026cef3)


## **获取View显示区域的百分比**
使用```getLocalVisibleRect```获取```Rect```判断```top```,```bottom```是顶部漏出还是底部漏出




## **Android打包流程**

### 打包资源文件，生成R.java文件

    打包资源的工具是AAPT（The Android Asset Packaing Tool），位于android-sdk/platform-tools目录下。在这个过程中，项目中的AndroidManifest.xml文件和布局文件XML都会编译，然后生成相应的R.java。 

### 处理aidl文件，生成相应的Java文件

    这一过程中使用到的工具是aidl（Android Interface Definition Language），即Android接口描述语言。位于android-sdk/platform-tools目录下。aidl工具解析接口定义文件然后生成相应的Java代码接口供程序调用。(如果在项目没有使用到aidl文件，则可以跳过这一步。)

### 编译项目源代码，生成class文件

    项目中所有的Java代码，包括R.java和.aidl文件，都会变Java编译器（javac）编译成.class文件，生成的class文件位于工程中的bin/classes目录下。

### 转换所有的class文件，生成classes.dex文件

    dx工具生成可供Android系统Dalvik虚拟机执行的classes.dex文件，该工具位于android-sdk/platform-tools 目录下。
    任何第三方的libraries和.class文件都会被转换成.dex文件。
    dx工具的主要工作是将Java字节码转成成Dalvik字节码、压缩常量池、消除冗余信息等。

### 打包生成APK文件

    所有没有编译的资源（如images等）、编译过的资源和.dex文件都会被apkbuilder工具打包到最终的.apk文件中。

    打包的工具apkbuilder位于 android-sdk/tools目录下。apkbuilder为一个脚本文件，实际调用的是android-sdk/tools/lib/sdklib.jar文件中的com.android.sdklib.build.ApkbuilderMain类。

### 对APK文件进行签名

    一旦APK文件生成，它必须被签名才能被安装在设备上。

    在开发过程中，主要用到的就是两种签名的keystore。一种是用于调试的debug.keystore，它主要用于调试，在Eclipse或者Android Studio中直接run以后跑在手机上的就是使用的debug.keystore。另一种就是用于发布正式版本的keystore。

### 对签名后的APK文件进行对齐处理

    如果你发布的apk是正式版的话，就必须对APK进行对齐处理，用到的工具是zipalign，它位于android-sdk/tools目录下。

    对齐的主要过程是将APK包中所有的资源文件距离文件起始偏移为4字节整数倍，这样通过内存映射访问apk文件时的速度会更快。

    对齐的作用就是减少运行时内存的使用。

---
  ![android打包流程](https://images2017.cnblogs.com/blog/357738/201708/357738-20170811145922054-1443365909.png)

![](https://img-blog.csdnimg.cn/20190716175841680.png#pic_center)

https://blog.csdn.net/wangzhongshun/article/details/96160984


## **apk安装流程**

- 复制APK到/data/app目录下，解压并扫描安装包。
- 资源管理器解析APK里的资源文件。
- 解析AndroidManifest文件，并在/data/data/目录下创建对应的应用数据目录。
- 然后对dex文件进行优化，并保存在dalvik-cache目录下。
- 将AndroidManifest文件解析出的四大组件信息注册到PackageManagerService中。
- 安装完成后，发送广播。

## **Activity生命周期**

### activity B正常主题
- activity A创建
```
A.onCreate -> A.onStart -> A.onResume
```

- activity A 打开 activity B
```
A.onPause -> B.onCreate -> B.onStart -> B.onResume -> A.onStop
```

- Activity B 按back键

```
B.onPause -> A.onRestart -> A.onStart -> A.onResume -> B.onStop -> B.onDestroy
```

### activity B窗口主题

- activity A 打开 activity B
```
A.onPause -> B.onCreate -> B.onStart -> B.onResume 
```

- Activity B 按back键

```
B.onPause -> A.onResume -> B.onStop -> B.onDestroy
```

## 切换横竖屏时，onCreate会调用吗？几次？
    程序在运行时，一些设备的配置可能会改变，如：横竖屏的切换、键盘的可用性或语言的切换等，此时Activity会重新启动。其中的过程是：在销毁之前会先调用onSaveInstancestate()去保存应用中的一些数据，然后调用 onDestory()，最后才会去调用onCreate()或者onRestoreInstanceState方法重新启动Activiy。在切换屏幕时候会重新调用各个生命周期，切横屏时会执行一次onCreate，切竖屏时会执行两次onCreate。





## **Handler**

### post()与sendMessage()区别
最终都会调用```sendMessageDelayed```方法传入```Message```参数，只是```post()```方法会设置```Message```的```callback```变量

### IdleHandler （闲时机制）

IdleHandler是一个回调接口，可以通过MessageQueue的addIdleHandler添加实现类。当MessageQueue中的任务暂时处理完了（没有新任务或者下一个任务延时在之后），这个时候会回调这个接口，返回false，那么就会移除它，返回true就会在下次message处理完了的时候继续回调。
调用地方在```MessageQueue.next()```

### Looper.loop()为什么不会阻塞主线程
```Android```是基于事件驱动的，即所有```Activity```的生命周期都是通过Handler事件驱动的。loop方法中会调用```MessageQueue```的```next```方法获取下一个```message```，当没有消息时，基于```Linux pipe/epoll```机制会阻塞在loop的```queue.next()```中的```nativePollOnce()```方法里，并不会消耗CPU。

### 同步屏障机制(sync barrier)

同步屏障可以通过MessageQueue.postSyncBarrier函数来设置。该方法发送了一个没有target的Message到Queue中，在next方法中获取消息时，如果发现没有target的Message，则在一定的时间内跳过同步消息，优先执行异步消息。再换句话说，同步屏障为Handler消息机制增加了一种简单的优先级机制，异步消息的优先级要高于同步消息。在创建Handler时有一个async参数，传true表示此handler发送的时异步消息。ViewRootImpl.scheduleTraversals方法就使用了同步屏障，保证UI绘制优先执行。

https://blog.csdn.net/asdgbc/article/details/79148180

### Message是什么样子的链表结构

[单链表结构](https://www.jianshu.com/p/94226b3f3ffb)


## **如何加载一张大图不oom**
- 获取大图尺寸
- 获取ImageVIew的尺寸，缩放减小内存
- 加载

https://www.cnblogs.com/billshen/p/13308650.html


## **如何优化卡顿、view卡顿分析**
- 检查是否是过渡绘制
    - 减少背景，如去除和列表背景色相同的Item背景色，子view和父view相同的背景等
        - 优化布局层级，使用merge,incloud标签，使用constantlayout替换布局等
        - 使用viewstub延迟布局加载
        -
- 使用工具分析
    - systrace 可以得到在UI绘制上是因为什么原因不符合Google制定的标准，比如measure/layout时间过长
    - traceview 得到每个方法执行的耗时，分析优化耗时的地方

## **okhttp excute 执行流程**


## **view绘制流程源码从哪开始的**
是从viewRootImpl.scheduleTraversals方法开始的,choreographer.post TraversalRunnable,在TraversalRunnable
里执行performTraversals方法view绘制


## **handler的异步消息怎么处理**
messagequeue.next方法读取到message的traget为null时就暂停获取同步消息的message，从队列里
获取异步消息最后返回到looper


## **handler流程**
handler发送message到messagequeue队列里，looper的loop方法里一直在循环的调用messagequeue的next方法获取message，
获取到message后根据message的target的handler发送到对应的dispatchMessage,在dispatchMessage方法里判断是runable还是
普通的message

## **出处&链接**

- https://www.cnblogs.com/1157760522ch/
- https://www.jianshu.com/p/5e5908ab3ea9

