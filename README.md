## 2017 Android 面试题集合，欢迎补充

* **说明**：部分内容来自网络，若有侵权请联系我删除

## Android - [Java](./Java.md)

### **基础知识**

* [Android基础知识](https://github.com/GeniusVJR/LearningNotes/blob/master/Part1/Android/Android%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86.md)    


### **Service生命周期**
* service的生命周期分为两种<br>

    ![生命周期](http://mmbiz.qpic.cn/mmbiz_png/CP9AlgoibiagV38ticvRln0KQkPmtfiaMEUf7IOqsByHdSY85RVD5vgibRldmZ9YzvfTDyjHT4sKib3anjCWnKdD8UZg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

### **Android中的几种动画**
* **补间动画** :是对某个View进行一系列的动画的操作，包括淡入淡出（Alpha），缩放（Scale），平移（Translate），旋转（Rotate）四种模式。

* **帧动画** : 将多张图片组合起来进行播放，类似于早期电影的工作原理，很多App的loading是采用这种方式

* **属性动画** : 属性动画不再仅仅是一种视觉效果了，而是一种不断地对值进行操作的机制，并将值赋到指定对象的指定属性上，可以是任意对象的任意属性

### **从ActivityA跳转到ActivityB的生命周期调用顺序**
* **打开ActivityA**<br>
    ```
    onCreate(A) -> onStart(A) -> onResume(A)
    ```

* **打开ActivityB**<br>
    ```
    onPause(A) -> onCreate(B) -> onStart(B) -> onResume(B) -> onStop(A)
    ```
* **回到ActivtyA** <br>
    ```
    onPause(B) -> onRestart(A) -> onStart(A) -> onResume(A) -> onStop(B) -> onDestory(B)
    ```

### **HandlerThread的原理**

* **释义** HandlerThread 是可以创建带有 looper 新线程的类
* 


### **判断处于主线程还是子线程**

```
//1
Looper.getMainLooper() == Looper.myLooper();

//2
Looper.getMainLooper().getThread() == Thread.currentThread();

//3
Looper.getMainLooper().getThread().getId() == Thread.currentThread().getId();
```

### **设计模式**

* [面向对象六大原则](https://github.com/francistao/LearningNotes/blob/master/Part1/DesignPattern/%E5%B8%B8%E8%A7%81%E7%9A%84%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E8%AE%BE%E8%AE%A1%E5%8E%9F%E5%88%99.md)
* [单例模式](https://github.com/francistao/LearningNotes/blob/master/Part1/DesignPattern/%E5%8D%95%E4%BE%8B%E6%A8%A1%E5%BC%8F.md)
* [Builder模式](https://github.com/GeniusVJR/LearningNotes/blob/master/Part1/DesignPattern/Builder%E6%A8%A1%E5%BC%8F.md)
* [原型模式](https://github.com/GeniusVJR/LearningNotes/blob/master/Part1/DesignPattern/%E5%8E%9F%E5%9E%8B%E6%A8%A1%E5%BC%8F.md)
* [简单工厂](https://github.com/francistao/LearningNotes/blob/master/Part1/DesignPattern/%E7%AE%80%E5%8D%95%E5%B7%A5%E5%8E%82.md)
* 工厂方法模式
* 抽象工厂模式
* [策略模式](https://github.com/GeniusVJR/LearningNotes/blob/master/Part1/DesignPattern/%E7%AD%96%E7%95%A5%E6%A8%A1%E5%BC%8F.md)
* 状态模式
* [责任链模式](https://github.com/GeniusVJR/LearningNotes/blob/master/Part1/DesignPattern/%E8%B4%A3%E4%BB%BB%E9%93%BE%E6%A8%A1%E5%BC%8F.md)
* 解释器模式
* 命令模式
* [观察者模式](https://github.com/GeniusVJR/LearningNotes/blob/master/Part1/DesignPattern/%E8%A7%82%E5%AF%9F%E8%80%85%E6%A8%A1%E5%BC%8F.md)
* 备忘录模式
* 迭代器模式
* 模板方法模式
* 访问者模式
* 中介者模式
* [代理模式](https://github.com/GeniusVJR/LearningNotes/blob/master/Part1/DesignPattern/%E4%BB%A3%E7%90%86%E6%A8%A1%E5%BC%8F.md)
* 组合模式
* [适配器模式](https://github.com/GeniusVJR/LearningNotes/blob/master/Part1/DesignPattern/%E9%80%82%E9%85%8D%E5%99%A8%E6%A8%A1%E5%BC%8F.md)
* 装饰模式
* 享元模式
* [外观模式](https://github.com/GeniusVJR/LearningNotes/blob/master/Part1/DesignPattern/%E5%A4%96%E8%A7%82%E6%A8%A1%E5%BC%8F.md)
* 桥接模式



### **onMeasure返回的两个参数都有什么信息**

* 两个参数的意义: 父控件对View的宽高约束
* http://blog.csdn.net/xmxkf/article/details/51490283

### **View绘制流程**

* **绘制过程**：View的绘制过程是从ViewRoot 的 performTraversals 方法开始的， 它经过 measure、layout 和 draw 三个过程才能最终将一个 View 绘制出来，其中 measure 用来测量 View 的宽高，layout 用来确定 view 在容器中的位置，draw 则负责将 view 绘制在屏幕上

* **流程图**

    ![流程图](http://img.blog.csdn.net/20150529090922419)

* **链接**
    * http://blog.csdn.net/guolin_blog/article/details/16330267
    * http://blog.csdn.net/yanbober/article/details/46128379

* **书籍参考**
    * 《Android开发艺术探索》第四章 P176


### **Activity的创建过程**

 ActivityManagerService -> </br>
 ActivityStackSupervisor -> </br>
 ActivityStack -> </br>
 ActivityStackSupervisor -> </br>
 ApplicationThread 

* **链接**
    * http://blog.csdn.net/feiduclear_up/article/details/49201357
    * http://blog.csdn.net/xmxkf/article/details/52452218
    * [Android源码分析-Activity的启动过程](http://blog.csdn.net/singwhatiwanna/article/details/18154335)

### **Handler**

* **Handler** 是Android类库提供的用于接受、传递和处理消息或Runnable对象的处理类，它结合Message、MessageQueue和Looper类以及当前线程实现了一个消息循环机制，用于实现任务的异步加载和处理
* **主要用途**

    * **执行定时任务**  指定任务时间，在某个具体时间或某个时间段后执行特定的任务操作，例如使用Handler提供的postDelayed(Runnable r,long delayMillis)方法指定在多久后执行某项操作

    * **线程间的通信**  在执行较为耗时的操作时，Handler负责将子线程中执行的操作的结果传递到UI线程，然后UI线程再根据传递过来的结果进行相关UI元素的更新

* **Handler用法**
    
    ![示意图](http://img.blog.csdn.net/20140825153914738)

* 根据上面的图片，我们现在来解析一下异步消息处理机制
    * **Message**：消息体，用于装载需要发送的对象

    * **handler**：它直接继承自Object。作用是：**在子线程中发送Message或者Runnable对象到MessageQueue中；在UI线程中接收、处理从MessageQueue分发出来的Message或者Runnable对象**。发送消息一般使用Handler的sendMessage()方法，而发出去的消息经过处理后最终会传递到Handler的handlerMessage()方法中

    * **MessageQueue**：**用于存放Message或Runnable对象的消息队列**。它由对应的Looper对象创建，并由Looper对象管理。每个线程中都只会有一个MessageQueue对象

    * **Looper**：是每个线程中的MessageQueue的管家，**循环不断地管理MessageQueue接收和分发Message或Runnable的工作**。调用Looper的loop()方法后，就会进入到一个无限循环中然后每当发现MessageQueue中存在一条消息，就会将它取出，并调用Handler的handlerMessage()方法。每个线程中也只会有一个Looper对象

* Handler和Looper对象是属于线程内部的数据，不过也提供与外部线程的访问接口，**Handler就是公开给外部线程的接口，用于线程间的通信**。Looper是由系统支持的用于创建和管理MessageQueue的依附于一个线程的循环处理对象，而Handler是用于操作线程内部的消息队列的，所以Handler也必须依附一个线程，而且只能是一个线程

* **链接**
    - http://www.jianshu.com/p/02962454adf7

### **解释下Application类**

* **定义**
    * 用于维护全局应用程序状态的基础类
    * 代表应用程序的类，也属于Android中的一个系统组件
    * 继承关系: 继承自 ContextWarpper 类

* **Application 与 Context**
  
    - Activity、Service、Application 都是 Context 的子类。Context 是一个抽象类，具体的实现是在 ContextImpl 类中。因此应用程序 APP 共用的 Context 数目公式为:
        ```
        context数 = Service数 + Actvity数 + 1(Application 对应的 Context 实例)
        ```

* **特点**
    * **实例创建方式**：单例模式
        - 每个 APP 运行时会首先自动创建 application 类并实例化 application 对象且只有一个（即 application 类是单例模式）
        - 可以通过继承 application 来实现自定义的 application 类

    * **实例形式**：全局模式，即不同组件都可以获得 application 对象且都是同一个

    * **生命周期**：等于 APP 的生命周期.

* **连接**
    * http://www.jianshu.com/p/f665366b2a47
    * http://blog.qiji.tech/archives/14085

### **APK安装过程**

*  **APK安装的主要步骤**
    - 将 apk 文件复制到 /data/app/ 目录下
    - 解压 apk ，拷贝文件， 创建应用的数据目录
    - 解析 apk 的AndroidManifinest.xml 文件
    - 显示快捷方式

* **链接**
    - http://www.androidchina.net/6667.html

### **MultiDex工作原理分析和优化方案**

* **链接**
    * https://zhuanlan.zhihu.com/p/24305296

### **应用启动流程分析**
* **链接**
    * [罗升阳 - Android应用程序启动过程源代码分析](http://blog.csdn.net/luoshengyang/article/details/6689748)
    * http://solart.cc/2016/08/20/launch_app/


### **View的测量宽/高和最终宽/高有什么区别（另一种问法: view 的 getMeasuredWidth 和 getWidth 有什么区别）**

* 在 View 的默认实现中 View 的测量宽/高和最终宽/高是相等的，只不过 测量宽高（getMeasureWidth/Height）是在 view 的 measure 过程中调用的，而 view 的 最终宽高（getWidth/Height）是在 view 的 layout 过程中调用的;即两者的赋值时机不同，测量宽高比最终宽高获取的时机稍微早点;

* 在一般开发中这两者的值是一样的，但是也会存在特殊情况，
    - 比如在 view 的 layout 方法中:
        ```
        public void layout(int l, int t, int r, int b){
             super.layout(l, t, r+100, b+100);
        }
        ```
        这个步骤就会导致测量宽高比最终宽高小 100px
    
    - 另一种情况是在某些情况下 View 需要多次 measure 才能确定自己的测量宽高，在前几次的测量宽高过程中得出的值可能和最终宽高的不一致，但最终来说：测量宽高和最终宽高相等


### **事件分发流程**
* **Android事件分发流程**
    ```
    Activity(Windwos) -> ViewGroup -> View
    ```
    ![流程图说明](http://upload-images.jianshu.io/upload_images/2001124-1e27c73c26652a84.png?imageMogr2/auto-orient/strip)

    * 对于```dispatchTouchEvent``` , ```onTouchEvent``` 返回 true 就是自己消费了，返回 false 就传到父View 的```onTouchEvent```方法
    * ViewGroup 想把事件分发给自己的 ```onTouchEvent```,需要在```onInterceptTouchEvent```方法中返回 true 把事件拦截下来
    * ViewGroup 的 ```onInterceptTouchEvent``` 默认不拦截，所以 ```super.onInterceptTouchEvent() = false```
    * View(这里指没有子View)没有拦截器,所以 View 的```dispatchTouchEvent```的```super.dispatchTouchEvent(event)```默认把事件分发给自己的```onTouchEvent```


* **链接**
    - [Android事件分发机制详解：史上最全面、最易懂](http://blog.csdn.net/carson_ho/article/details/54136311)
    - [图解 Android 事件分发机制](http://www.jianshu.com/p/e99b5e8bd67b)

### **View的渲染机制**
* **链接**
    * [Android性能优化第（四）篇---Android渲染机制](http://www.jianshu.com/p/9ac245657127)

### **编译打包的过程**
* 等待补充

### **ANR的原理(源码角度)**
* 等待补充

### **属性动画的原理**
* 等待补充

### **Android有多个资源文件夹，应用在不同分辨率下是如何查找对应文件夹下的资源的，描述整个过程**
* 等待补充

### **应用最多占可被分配多少内存**

-> 进程数*16M

### **一个应用中有多少个 Window**
* 有视图的地方就有 Window，比如 Activity、Dialog、Toast、PopUpWindows、菜单 等视图都对应着一个window.

* **连接**
    * [ Android 带你彻底理解 Window 和 WindowManager](http://blog.csdn.net/yhaolpz/article/details/68936932)


### **为什么Dialog不能用Application的Context**
* Dialog初化始时是通过Context.getSystemServer 来获取 WindowManager，而如果用Application或者Service的Context去获取这个WindowManager服务的话，会得到一个WindowManagerImpl的实例，这个实例里token也是空的。之后在Dialog的show方法中将Dialog的View(PhoneWindow.getDecorView())添加到WindowManager时会给token设置默认值还是null。
如果这个Context是Activity，则直接返回Activity的mWindowManager，这个mWindowManager在Activity的attach方法被创建，Token指向此Activity的Token，mParentWindow为Activity的Window本身

* **答案**<br>
 那为什么一定要是Activity的Token呢？我想使用Token应该是为了安全问题，通过Token来验证WindowManager服务请求方是否是合法的。如果我们可以使用Application的Context，或者说Token可以不是Activity的Token，那么用户可能已经跳转到别的应用的Activity界面了，但我们却可以在别人的界面上弹出我们的Dialog，想想就觉得很危险。


* **连接** <br>
     http://www.jianshu.com/p/628ac6b68c15

### **Android Activity 、 Window 、 View之间的关系**

* 如图

    ![](http://img.blog.csdn.net/20151030181018072?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

### **关于Android Force Close 出现的原因 以及解决方法**
* **原因** 
    * Error
    * OOM , 内存溢出
    * StackOverFlowError
    * RuntimeException(比如空指针异常)

* **如何避免** <br>
    * 可以实现Thread.UncaughtExceptionHandler接口的uncaughtException方法 

###  **哪些情况会出现内存泄漏，如何解决**
* 对于使用了Boardcast Receive、ContentObserver、File、Cursor、Stream、Bitmap等资源的时候没有销毁。<br>
  解决方法 -> 应该在不使用的时候关闭或者注销

* 静态内部类持有外部成员变量<br>
  解决方法 -> 可以使用若引用
    
* 使用了 context 持有 Activity 导致 Activity无法释放 <br>
  解决方法 -> 使用 ApplicationContext 
    
* 集合中没有使用的对象没有及时 remove

* handler 引起的内存泄漏 <br>
  解决方法 -> 使用若引用持有 Activity等的 Context
    
* 设置过的Listener没有及时移除 <br>
  解决方法 -> 在destory 里 设置 Listener 为 null


### **Android系统的架构**

![图](http://upload-images.jianshu.io/upload_images/2893137-1047c70c15c1589b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## **关于第三库问题**
---

### **Glide4.0源码解析**
* **链接**
    * [Android 图片加载框架Glide4.0源码完全解析 一](http://www.cnblogs.com/guanmanman/p/7008259.html)
    * [Android 图片加载框架Glide4.0源码完全解析 二](http://www.cnblogs.com/guanmanman/p/7040942.html)

### **otto源码解析**

* [otto](https://github.com/square/otto) 这个开源项目是一个event bus模式的消息框架，用于程序各个模块之间的通信，此消息框架可以使得各个
模块之间减少耦合性。

* **链接**
    * [otto源码分析](http://blog.csdn.net/com360/article/details/38640771)

### **Gilde怎么实现圆角图**
* **实现原理**  利用 ```Transform```
* **代码示例**
    ```
    public class GlideRoundTransform extends BitmapTransformation {

    private static float radius = 0f;

    /**
     * 构造函数 默认圆角半径 4dp
     *
     * @param context Context
     */
    public GlideRoundTransform(Context context) {
        this(context, 4);
    }

    /**
     * 构造函数
     *
     * @param context Context
     * @param dp      圆角半径
     */
    public GlideRoundTransform(Context context, int dp) {
        super(context);
        radius = Resources.getSystem().getDisplayMetrics().density * dp;
    }

    @Override
    protected Bitmap transform(BitmapPool pool, Bitmap toTransform, int outWidth, int outHeight) {
        return roundCrop(pool, toTransform);
    }

    private static Bitmap roundCrop(BitmapPool pool, Bitmap source) {
        if (source == null) return null;

        Bitmap result = pool.get(source.getWidth(), source.getHeight(), Bitmap.Config.ARGB_8888);
        if (result == null) {
            result = Bitmap.createBitmap(source.getWidth(), source.getHeight(), Bitmap.Config.ARGB_8888);
        }

        Canvas canvas = new Canvas(result);
        Paint paint = new Paint();
        paint.setShader(new BitmapShader(source, BitmapShader.TileMode.CLAMP, BitmapShader.TileMode.CLAMP));
        paint.setAntiAlias(true);
        RectF rectF = new RectF(0f, 0f, source.getWidth(), source.getHeight());
        canvas.drawRoundRect(rectF, radius, radius, paint);
        return result;
    }

    @Override
    public String getId() {
        return getClass().getName() + Math.round(radius);
    }
  }
    ```


## **其他链接**
---
* [Android 开发工程师面试指南](https://www.diycode.cc/wiki/androidinterview)

* [Android 开发面试 “108” 问](https://www.diycode.cc/topics/993?utm_source=gank.io&utm_medium=email)

* [这些Android面试题你一定需要](https://mp.weixin.qq.com/s?__biz=MzI0MjE3OTYwMg==&mid=2649548612&idx=1&sn=8e46b6dd47bd8577a5f7098aa0889098&chksm=f1180c39c66f852fd955a29a9cb4ffa9dc4d528cab524059bcabaf37954fa3f04bc52c41dae8&scene=21#wechat_redirect)


 
