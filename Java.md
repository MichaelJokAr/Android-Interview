> **说明**：部分内容来自网络，若有侵权请联系我删除

# Java - [Android](./README.md) - [网络](./network.md)

## **ArrayList和LinkedList的区别**

* ArrayList是实现了基于动态数组的数据结构，LinkedList基于链表的数据结构
* 对于随机访问get和set，ArrayList觉得优于LinkedList，因为LinkedList要移动指针
* 对于新增和删除操作add和remove，LinedList比较占优势，因为ArrayList要移动数据

* 性能对比
     - 对ArrayList和LinkedList而言，在列表末尾增加一个元素所花的开销都是固定的。对ArrayList而言，主要是在内部数组中增加一项，指向所添加的元素，偶尔可能会导致对数组重新进行分配；而对LinkedList而言，这个开销是统一的，分配一个内部Entry对象
    - 在ArrayList的中间插入或删除一个元素意味着这个列表中剩余的元素都会被移动；而在LinkedList的中间插入或删除一个元素的开销是固定的
    - LinkedList不支持高效的随机元素访问
    - ArrayList的空间浪费主要体现在在list列表的结尾预留一定的容量空间，而LinkedList的空间花费则体现在它的每一个元素都需要消耗相当的空间

* 当操作是在一列数据的后面添加数据而不是在前面或中间,并且需要随机地访问其中的元素时,使用ArrayList会提供比较好的性能；当你的操作是在一列数据的前面或中间添加或删除数据,并且按照顺序访问其中的元素时,就应该使用LinkedList了

- http://pengcqu.iteye.com/blog/502676

## **Java有几种Map**

* HashMap
* Hashtable
* Properties
* LinkedHashMap
* IdentityHashMap
* TreeMap
* WeakHashMap
* ConcurrentHashMap

## **HashMap是不是有序的，哪些Map是有序的**

* HashMap是无序的
* LinkedHashMap 和 TreeMap 都是有序Map
    - TreeMap 的顺序是按照他自己的算法排序的不能指定
    - LinkedHashMap 的顺序是按照输入的顺序排列

## **list与map -底层如何实现**
* **Collection(单列集合)**
    * **List(有序,可重复)** <br>
        * **ArrayList**
            * 底层数据结构是数组，查询快，增删慢
            * 线程不安全，效率高
        * **Vertor**
            * 底层数据结构是数组,查询快,增删慢
            * 线程安全，效率低
        * **LinkedList**
            * 底层数据结构是双向链表,查询慢,增删快
            * 线程不安全，效率高
    * **Set(无序，唯一)**
        * **HashSet**
            * 底层数据结构是哈希表。
            * 哈希表依赖两个方法：```hashCode()```和```equals()```<br>
            * 执行顺序：
                * 首先判断```hashCode()```值是否相同
                    * 是：继续执行```equals()```,看其返回值： 是 true 说明元素重复，不添加；是 false 就直接添加到集合
                    * 否：就直接添加到集合
                * 最终：自动生成```hashCode()```和```equals()```即可
        * **LinkedHashSet**
            * 底层数据结构由链表和哈希表组成
            * 由链表保证元素有序
            * 由哈希表保证元素唯一
        * **TreeSet**
            * 底层数据结构是红黑树(是一种自平衡的二叉树)
            * 根据比较的返回值是否为0来决定元素的唯一性
            * 元素排序：
                * 自然排序（元素具备比较性）：让元素所属的类实现```Comparable``接口
                * 比较器排序（集合具备比较性）：让集合接收一个```Comparable```的实现类对象
* Map(双列集合)
    * Map 集合的数据结构仅仅对键有效，与值无关
    * 存储的是键值形式的元素，键唯一，值可重复

    * **HashMap**
        * 底层数据结构是哈希表，线程不安全，效率高
            * 哈希表依赖两个方法：```hashCode()```和```equals()```
                * 执行顺序：
                    * 首先判断```hashCode()```值是否相同
                        * 是：继续执行```equals()```,看其返回值：true 说明元素重复，不添加；false 直接添加到集合
                        * 否：直接添加到集合
                    * 最终：自动生成```hashCode()```和```equals()```两个方法
    * **LinkedHashMap**
        * 底层数据结构是哈希表和链表组成
        * 由链表保证顺序
        * 由哈希保证唯一性
    * **Hashtable**
        * 底层数据结构是哈希表，线程安全，效率低
            * 哈希表依赖两个方法：```hashCode()```和```equals()```
                * 执行顺序：
                    * 首先判断```hashCode()```值是否相同
                        * 是：继续执行```equals()```,看其返回值：true 说明元素重复，不添加；false 直接添加到集合
                        * 否：直接添加到集合
                    * 最终：自动生成```hashCode()```和```equals()```两个方法
    * **TreepMap**
        * 底层数据结构是红黑树(是一直自平衡的二叉树)
        * 根据比较的返回值是否为0来确定唯一性
        * 元素排序：
            * 自然排序(元素具备比较性)：让元素所属类实现```Comparable```接口
            * 比较器排序(集合具备比较性)：让集合接收一个```Comparable```的实现类对象

* **链接**
    * http://blog.csdn.net/xy2953396112/article/details/54891527

## **synchronized关键字以及锁的等级：方法锁、对象锁、类锁**
* 方法锁和对象锁说的其实是一个东西，即只有方法锁或对象锁 和类锁两种锁

* **内置锁**：每个java对象都可以用做一个实现同步的锁，这些锁成为内置锁。线程进入同步代码块或方法的时候会自动获得该锁，在退出同步代码块或方法时会释放该锁。获得内置锁的唯一途径就是进入这个锁的保护的同步代码块或方法。<br>
内置锁是一个互斥锁，这就是意味着最多只有一个线程能够获得该锁，当线程A尝试去获得线程B持有的内置锁时，线程A必须等待或者阻塞，知道线程B释放这个锁，如果B线程不释放这个锁，那么A线程将永远等待下去。

* **对象锁和类锁**：Java的对象锁和类锁在锁的概念上基本上和内置锁是一致的，但是，两个锁实际是有很大的区别的，对象锁是用于对象实例方法，或者一个对象实例上的，类锁是用于类的静态方法或者一个类的class对象上的。我们知道，类的对象实例可以有很多个，但是每个类只有一个class对象，所以不同对象实例的对象锁是互不干扰的，但是每个类只有一个类锁。**但是有一点必须注意的是，其实类锁只是一个概念上的东西，并不是真实存在的，它只是用来帮助我们理解锁定实例方法和静态方法的区别的**

* **链接**
    * http://zhh9106.iteye.com/blog/2151791


## **Java有哪几种引用**

* **强引用**：平时我们编程的时候例如：Object object=new Object（）；那object就是一个强引用了。如果一个对象具有强引用，那就类似于必不可少的生活用品，垃圾回收器绝不会回收它。当内存空 间不足，Java虚拟机宁愿抛出OutOfMemoryError错误，使程序异常终止，也不会靠随意回收具有强引用的对象来解决内存不足问题。

* **软引用(SoftReference)**：如果一个对象只具有软引用，那就类似于可有可物的生活用品。如果内存空间足够，垃圾回收器就不会回收它，如果内存 空间不足了，就会回收这些对象的内存。只要垃圾回收器没有回收它，该对象就可以被程序使用。软引用可用来实现内存敏感的高速缓存。 软引用可以和一个引用队列（ReferenceQueue）联合使用，如果软引用所引用的对象被垃圾回收，Java虚拟机就会把这个软引用加入到与之关联 的引用队列中

* **弱引用(WeakReference)**：如果一个对象只具有弱引用，那就类似于可有可物的生活用品。弱引用与软引用的区别在于：只具有弱引用的对象拥有更 短暂的生命周期。在垃圾回收器线程扫描它 所管辖的内存区域的过程中，一旦发现了只具有弱引用的对象，不管当前内存空间足够与否，都会回收它的内存。不过，由于垃圾回收器是一个优先级很低的线程， 因此不一定会很快发现那些只具有弱引用的对象。 弱引用可以和一个引用队列（ReferenceQueue）联合使用，如果弱引用所引用的对象被垃圾回收，Java虚拟机就会把这个弱引用加入到与之关联 的引用队列中

* **虚引用(PhantomReference)**：“虚引用”顾名思义，就是形同虚设，与其他几种引用都不同，虚引用并不会决定对象的生命周期。如果一个对象 仅持有虚引用，那么它就和没有任何引用一样，在任何时候都可能被垃圾回收。 虚引用主要用来跟踪对象被垃圾回收的活动。虚引用与软引用和弱引用的一个区别在于：虚引用必须和引用队列 （ReferenceQueue）联合使用。当垃圾回收器准备回收一个对象时，如果发现它还有虚引用，就会在回收对象的内存之前，把这个虚引用加入到与之 关联的引用队列中。程序可以通过判断引用队列中是否已经加入了虚引用，来了解被引用的对象是否将要被垃圾回收。程序如果发现某个虚引用已经被加入到引用队 列，那么就可以在所引用的对象的内存被回收之前采取必要的行动

## **接口的所有功能抽象类能实现吗**

* 不能，class可以继承多个interface，而只能继承一个abstract class
* https://www.ibm.com/developerworks/cn/java/l-javainterface-abstract/index.html


## **String、StringBuffer、StringBuilder 区别**
* **String** 是不可变类，任何对 String 的改变都会引发新的 String 对象生成

* **StingBuffer** 是可变类，任何对他所指代的字符串的改变都会产生新的对象，支持并发操作，线程安全的。适合多线程中使用

* **StringBuilder** 是可变类，但不支持并发操作，线性不安全的，不适合在多线程中使用。但在单线程中性能比 StringBuffer 高

## **volatile详解**

* **volatile关键字的两层语义**
    * 保证了不同线程对这个变量进行操作时的可见性，即一个线程修改了某个变量的值，这新值对其他线程来说是立即可见的
    * 禁止进行指令重排序

* **链接**
    * http://www.cnblogs.com/dolphin0520/p/3920373.html

## **静态内部类和非静态内部类的不同**

* 内部静态类不需要指向外部类引用，但是非静态内部类需要持有外部类引用

* 非静态内部类能够访问外部类的静态和非静态成员。静态内部类不能访问非静态成员，他只能访问外部静态成员

* 一个非静态内部类不能脱离外部类实体被创建，一个非静态内部类可以访问外部类的数据和方法，因为他就在外部类里面。

## **Set和List的区别**

## **equals 和 == 的区别**
 * **首先说一下 Java 中的数据类型**：
    * 基本类型，也称原始数据类型：byte、short、chat、int、long、float、double、boolean
    * 复合数据类型（类）

* **```== ```**
    * 比较 基本类型时 比较的是他们的值
    * 比较复合数据类型时 比较的是他们在内存中存放的地址

* **equals**
    * 基本类型 无法用 equals
    * 复合类型：在 Java 中所有的类都是继承了 Object 这个基类，在Object中定义了一个 equals 的方法，这方法初始行为是比较对象的内存地址，但一些类库对他进行了重写比如 String、Integer、Date等都有自己的具体事项，这些不在是比较类在内存中的存放地址。<br>
     所以对复合数据类型之间进行 equals 比较：在没有重写 equals 方法情况下他们之间比较的还是在内存中的存放地址，因为Objcet的equals方法也是用 ```== ``` 进行比较的，所以比较结果跟 ```== ``` 的结果相同

## **Hashmap的原理**
* **原理** ： HashMap 是基于 hashing 的原理，我们使用 put(key,value) 存储对象到 HashMap 中，使用 get(key) 从 HashMap 中获取对象。当我们给 put() 方法传递键和值时，我们先对 key 调用 hashCode() 方法，放回的 hashCode 用于找到 bucket 位置来存储 Entry 对象

* **当两个对象的hashcode相同会发生什么？** ：因为 hashCode相同，所以他们的 bucket 位置也相同，'碰撞' 会发生。因为 HashMap 使用链表存储对象，这个 Entry(包含有键值对的 Map.Entry对象)会存储在链表中

* **如果两个键的hashcode相同，如何获取值对象** :  当我们调用 get() 方法，HashMap 会使用键对象的 hashCode 找到 bucket 位置，然后调用 keys.equlas() 方法去找到链表中正确节点，最终找到要找的值

## **用什么算法算出hashcode和数组链表对应的**
* hash算法

---
## **synchronized**
https://www.cnblogs.com/toria/p/11234323.html

## **synchronized  底层实现**

## **synchronized用的锁是存在哪里的？**

　　synchronized用到的锁是存在Java对象头里的。

## **synchronized和 Lock 的区别？**

- Lock是一个接口，而synchronized是Java中的关键字，synchronized是内置的语言实现；

- synchronized在发生异常时，会自动释放线程占有的锁，因此不会导致死锁现象发生；而Lock在发生异常时，如果没有主动通过unLock()去释放锁，则很可能造成死锁现象，因此使用Lock时需要在finally块中释放锁；

- Lock可以让等待锁的线程响应中断，而synchronized却不行，使用synchronized时，等待的线程会一直等待下去，不能够响应中断；

- 通过Lock可以知道有没有成功获取锁（tryLock()方法：如果获取锁成功，则返回true），而synchronized却无法办到。

- Lock可以提高多个线程进行读操作的效率。

　　在性能上来说，如果竞争资源不激烈，两者的性能是差不多的，而当竞争资源非常激烈时（即有大量线程同时竞争），此时Lock的性能要远远优于synchronized。所以说，在具体使用时要根据适当情况选择。

## **Q: 为什么单例模式用了synchroniezed 还要用volatile**


考虑这种情况，就知道原因了:若指令被编译器优化，

- （1）那么当A线程执行到instance = new Singleton();的时候，instance已经不为空了，但是对象其实是没有好的
- （2）当B线程执行到第一个if (instance == null) 时候，会判断instance已经有了，就直接拿去用，那么其实这个对象还没好呢，一定会出错的。

总结一下：如果两个线程都在同不块内 ，那么不加volatile是没问题的，但是当一个线程进入到同不块内，另一个还在外层判空的时候，就出问题了。

拓展：除了编译器做的指令优化之外，其实cpu也有乱序的执行的能力，那么可能这个instance也是没有被创建好就被使用的。

## volatile关键字的作用、原理

### 作用
- 防重排序
```
实例化一个对象其实可以分为三个步骤： 
　　（1）分配内存空间。 
　　（2）初始化对象。 
　　（3）将内存空间的地址赋值给对应的引用。 
但是由于操作系统可以对指令进行重排序，所以上面的过程也可能会变成如下过程： 
　　（1）分配内存空间。 
　　（2）将内存空间的地址赋值给对应的引用。 
　　（3）初始化对象 
　　如果是这个流程，多线程环境下就可能将一个未初始化的对象引用暴露出来，从而导致不可预料的结果。因此，为了防止这个过程的重排序，我们需要将变量设置为volatile类型的变量。
```
- 实现可见性
- 保证原子性


#### volatile的原理
- 可见性实现
```
线程本身并不直接与主内存进行数据的交互，而是通过线程的工作内存来完成相应的操作。这也是导致线程间数据不可见的本质原因。因此要实现volatile变量的可见性，直接从这方面入手即可。对volatile变量的写操作与普通变量的主要区别有两点： 
　　（1）修改volatile变量时会强制将修改后的值刷新的主内存中。 
　　（2）修改volatile变量后会导致其他线程工作内存中对应的变量值失效。因此，再读取该变量值的时候就需要重新从读取主内存中的值。 
```

- 有序性实现


>参考文档
- https://www.cnblogs.com/william-dai/p/10895949.html

## **Q: volatile关键字与synchroniezed关键字在内存的区别**

- volatile是变量修饰符，而synchronized则作用于一段代码或方法。

- volatile只是在线程内存和“主”内存间同步某个变量的值；而synchronized通过锁定和解锁某个监视器同步所有变量的值。显然synchronized要比volatile消耗更多资源。


[https://blog.csdn.net/qq360514136/article/details/88253255](https://blog.csdn.net/qq360514136/article/details/88253255)


## 下面的代码， str 值最终为多少？换成 Integer 值又为多少，是否会被改变？
```
public void test() {
    String str = "123";
    changeValue(str); 
    System.out.println("str值为: " + str);  // str未被改变，str = "123"
}

public changeValue(String str) {
    str = "abc";
}
```

- 考点：Java 值传递 (第 2 题相同)。编写代码测试，在 changeValue() 方法中修改入参，并不会改变之前的值；

- 原理 ：Java 程序设计语言总是采用按值调用，方法得到的是所有参数值的一个拷贝，即方法不能修改传递给它的任何参数变量的内容。基本类型参数传递的是参数副本，对象类型参数传递的是对象地址的副本；

- 题解：在 changeValue() 中，**对于对象类型参数，直接修改的是对象地址副本的值，所以之前变量的地址并未被修改！若修改的是对象实例里面的某个值，之前变量则会被修改**


## try...catch...finally与return
- 场景1不走catch
```
 try {
            return 1
        } catch (E: Exception) {
            return 2
        } finally {
            return 3
        }

```
先执行try然后执行finally，最终返回3
- 场景2走catch
```
        try {
            throw RuntimeException()
            return 1
        } catch (E: Exception) {
            return 2
        } finally {
            return 3
        }
```
执行catch然后执行finally，最终返回3

- 场景3有变量值更改-不走catch
```
//x传入2
    fun testFinally(x: Int): Int {
        var y = x
        try {
            y = y + 1
//            throw RuntimeException()
            return y
        } catch (E: Exception) {
            y = y + 2
            return y
        } finally {
            y = y + 3
            return y
        }
    }
```
先走try，y值变为3，然后执行finally，y值变为6，方法返回6

- 场景4有变量值更改走catch
```
//x传入2
    fun testFinally(x: Int): Int {
        var y = x
        try {
            y = y + 1
            throw RuntimeException()
            return y
        } catch (E: Exception) {
            y = y + 2
            return y
        } finally {
            y = y + 3
            return y
        }
    }
```
先走try，y值变为3，然后执行catch y值变为5，最后执行finally，y值变为8，方法返回8

- 场景5有变量值更改走catch，但是finally没有return
```
//x传入2
    fun testFinally(x: Int): Int {
        var y = x
        try {
            y = y + 1
            throw RuntimeException()
            return y
        } catch (E: Exception) {
            y = y + 2
            return y
        } finally {
            y = y + 3

        }
    }
```

先走try，y值变为3，然后执行catch y值变为5，最后执行finally，y值变为8，**但是方法返回值是5**


---

>结论：finally肯定会走（除非是System.exit()），不管有没有rerun。且有变量值时会保存上一步骤的改变

## static
- static 类可以继承
- static 方法不可以继承


## Java方法签名
>方法名和形参数据类型列表可以唯一的确定一个方法，与方法的返回值一点关系都没有，这是判断重载重要依据

https://blog.csdn.net/qiuchengjia/article/details/52910884

## == 与 equals 区别
- ```==``` 操作比较的是两个变量的值是否相等，对于引用型变量表示的是两个变量在堆中存储的地址是否相同，即栈中的内容是否相同。

- equals操作表示的两个变量是否是对同一个对象的引用，即堆中的内容是否相同。
- ```==``` 一般用在基本数据类型中，equals()一般比较字符串是否相等

## ClassLoader 的双亲委派机制

```
    类加载器的双亲委派机制的作用主要是能够保障Java平台的安全性。

　　在这委托机制中，除了JVM自带的根类加载器以外，其余的加载器都有且只有一个父加载器。当Java程序请求加载器loader加载example类时，loader会首先委托父加载器去加载该类，如果父类加载器不能加载，则父类加载器再向自己的父类加载器委托，如果直到根类加载器都无法加载，则由loader自己加载。

　　如果在向上委托的过程中，父加载器能加载成功，则返回example类的对应的Class对象的引用返回给loader。

```

## HashMap的工作原理以及代码实现，为什么要转换成红黑树？
![](https://img2018.cnblogs.com/blog/1628624/201905/1628624-20190507151215694-307751641.png)
- 为什么当桶中键值对数量大于8才转换成红黑树，数量小于6才转换成链表？
```
    HashMap在JDK1.8及以后的版本中引入了红黑树结构，若桶中链表元素个数大于等于8时，链表转换成树结构；若桶中链表元素个数小于等于6时，树结构还原成链表。因为红黑树的平均查找长度是log(n)，长度为8的时候，平均查找长度为3，如果继续使用链表，平均查找长度为8/2=4，这才有转换为树的必要。链表长度如果是小于等于6，6/2=3，虽然速度也很快的，但是转化为树结构和生成树的时间并不会太短。

    还有选择6和8，中间有个差值7可以有效防止链表和树频繁转换。假设一下，如果设计成链表个数超过8则链表转换成树结构，链表个数小于8则树结构转换成链表，如果一个HashMap不停的插入、删除元素，链表个数在8左右徘徊，就会频繁的发生树转链表、链表转树，效率会很低
```

## ConcurrentHashMap
https://blog.csdn.net/weixin_43185598/article/details/87938882

### 为什么ConcurrentHashMap是线程安全的?
```
JDK1.7中，ConcurrentHashMap使用的锁分段技术，将数据分成一段一段的Segment存储，然后给每一段数据配一把锁，当一个线程占用锁访问其中一个段数据的时候，其他段的数据也能被其他线程访问。
```

### JDK1.7中Segment的原理
```
Segment，它继承了ReentrantLock，具备锁和释放锁的功能。ConcurrentHashMap只有16个Segment，并且不会扩容，最多可以支持16个线程并发写。
```

### JDK1.8的ConcurrentHashMap怎么实现线程安全的?有什么好处呢?
```
JDK1.8放弃了锁分段的做法，采用CAS和synchronized方式处理并发。以put操作为例，CAS方式确定key的数组下标，synchronized保证链表节点的同步效果。
```
好处：
- 减少内存开销

   ``` 
   假设使用可重入锁，那么每个节点都需要继承AQS，但并不是每个节点都需要同步支持，只有链表的头节点（红黑树的根节点）需要同步，这无疑消耗巨大内存。
   ```
- 获得JVM的支持
    ```
    可重入锁毕竟是API级别的，后续的性能优化空间很小。synchronized则是JVM直接支持的，JVM能够在运行时作出相应的优化措施：锁粗化、锁消除、锁自旋等等。使得synchronized能够随着JDK版本的升级而不改动代码的前提下获得性能上的提升。
    ```

### 为什么不推荐使用HashTable呢
    HashTable容器使用synchronized来保证线程安全，但在线程竞争激烈的情况下HashTable的效率非常低下。因为多个线程访问HashTable的同步方法时，可能会进入阻塞或轮询状态。如线程1使用put进行添加元素，线程2不但不能使用put方法添加元素，并且也不能使用get方法来获取元素，所以竞争越激烈效率越低。

>[参考](https://www.cnblogs.com/xiaoyangjia/p/11598094.html)




## **Q: 线程原理**


## **Q:哪些可以作为gc root**
在Java语言里，可作为GC Roots对象的包括如下几种： 
- 虚拟机栈(栈桢中的本地变量表)中的引用的对象 
- 方法区中的类静态属性引用的对象 
- 方法区中的常量引用的对象 
- 本地方法栈中JNI的引用的对象
