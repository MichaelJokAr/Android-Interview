## 2017 Android - Java 面试题集合，欢迎补充
* **说明**：部分内容来自网络，若有侵权请联系我删除

## Java - [Android](./README.md)

### **ArrayList和LinkedList的区别**

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

### **Java有几种Map**

* HashMap
* Hashtable
* Properties
* LinkedHashMap
* IdentityHashMap
* TreeMap
* WeakHashMap
* ConcurrentHashMap

### **HashMap是不是有序的，哪些Map是有序的**

* HashMap是无序的
* LinkedHashMap 和 TreeMap 都是有序Map
    - TreeMap 的顺序是按照他自己的算法排序的不能指定
    - LinkedHashMap 的顺序是按照输入的顺序排列

### **Java有哪几种引用**

* **强引用**：平时我们编程的时候例如：Object object=new Object（）；那object就是一个强引用了。如果一个对象具有强引用，那就类似于必不可少的生活用品，垃圾回收器绝不会回收它。当内存空 间不足，Java虚拟机宁愿抛出OutOfMemoryError错误，使程序异常终止，也不会靠随意回收具有强引用的对象来解决内存不足问题。

* **软引用(SoftReference)**：如果一个对象只具有软引用，那就类似于可有可物的生活用品。如果内存空间足够，垃圾回收器就不会回收它，如果内存 空间不足了，就会回收这些对象的内存。只要垃圾回收器没有回收它，该对象就可以被程序使用。软引用可用来实现内存敏感的高速缓存。 软引用可以和一个引用队列（ReferenceQueue）联合使用，如果软引用所引用的对象被垃圾回收，Java虚拟机就会把这个软引用加入到与之关联 的引用队列中

* **弱引用(WeakReference)**：如果一个对象只具有弱引用，那就类似于可有可物的生活用品。弱引用与软引用的区别在于：只具有弱引用的对象拥有更 短暂的生命周期。在垃圾回收器线程扫描它 所管辖的内存区域的过程中，一旦发现了只具有弱引用的对象，不管当前内存空间足够与否，都会回收它的内存。不过，由于垃圾回收器是一个优先级很低的线程， 因此不一定会很快发现那些只具有弱引用的对象。 弱引用可以和一个引用队列（ReferenceQueue）联合使用，如果弱引用所引用的对象被垃圾回收，Java虚拟机就会把这个弱引用加入到与之关联 的引用队列中

* **虚引用(PhantomReference)**：“虚引用”顾名思义，就是形同虚设，与其他几种引用都不同，虚引用并不会决定对象的生命周期。如果一个对象 仅持有虚引用，那么它就和没有任何引用一样，在任何时候都可能被垃圾回收。 虚引用主要用来跟踪对象被垃圾回收的活动。虚引用与软引用和弱引用的一个区别在于：虚引用必须和引用队列 （ReferenceQueue）联合使用。当垃圾回收器准备回收一个对象时，如果发现它还有虚引用，就会在回收对象的内存之前，把这个虚引用加入到与之 关联的引用队列中。程序可以通过判断引用队列中是否已经加入了虚引用，来了解被引用的对象是否将要被垃圾回收。程序如果发现某个虚引用已经被加入到引用队 列，那么就可以在所引用的对象的内存被回收之前采取必要的行动

### **接口的所有功能抽象类能实现吗**

* 不能，class可以继承多个interface，而只能继承一个abstract class
* https://www.ibm.com/developerworks/cn/java/l-javainterface-abstract/index.html


### **String、StringBuffer、StringBuilder 区别**
* **String** 是不可变类，任何对 String 的改变都会引发新的 String 对象生成

* **StingBuffer** 是可变类，任何对他所指代的字符串的改变都会产生新的对象，支持并发操作，线程安全的。适合多线程中使用

* **StringBuilder** 是可变类，但不支持并发操作，线性不安全的，不适合在多线程中使用。但在单线程中性能比 StringBuffer 高

### **volatile详解**

* **volatile关键字的两层语义**
    * 保证了不同线程对这个变量进行操作时的可见性，即一个线程修改了某个变量的值，这新值对其他线程来说是立即可见的
    * 禁止进行指令重排序

* **链接**
    * http://www.cnblogs.com/dolphin0520/p/3920373.html

### **静态内部类和非静态内部类的不同**

* 内部静态类不需要指向外部类引用，但是非静态内部类需要持有外部类引用

* 非静态内部类能够访问外部类的静态和非静态成员。静态内部类不能访问非静态成员，他只能访问外部静态成员

* 一个非静态内部类不能脱离外部类实体被创建，一个非静态内部类可以访问外部类的数据和方法，因为他就在外部类里面。

### **Set和List的区别**

### **equals 和 == 的区别**
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

### **Hashmap的原理**
* **原理** ： HashMap 是基于 hashing 的原理，我们使用 put(key,value) 存储对象到 HashMap 中，使用 get(key) 从 HashMap 中获取对象。当我们给 put() 方法传递键和值时，我们先对 key 调用 hashCode() 方法，放回的 hashCode 用于找到 bucket 位置来存储 Entry 对象

* **当两个对象的hashcode相同会发生什么？** ：因为 hashCode相同，所以他们的 bucket 位置也相同，'碰撞' 会发生。因为 HashMap 使用链表存储对象，这个 Entry(包含有键值对的 Map.Entry对象)会存储在链表中

* **如果两个键的hashcode相同，如何获取值对象** :  当我们调用 get() 方法，HashMap 会使用键对象的 hashCode 找到 bucket 位置，然后调用 keys.equlas() 方法去找到链表中正确节点，最终找到要找的值

### **用什么算法算出hashcode和数组链表对应的**
* hash算法

### **拆箱装箱**