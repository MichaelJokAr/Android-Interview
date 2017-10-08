## 2017 Android 面试题集合，欢迎补充

## Android - [Java](./Java.md)

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

* http://www.cnblogs.com/android-blogs/p/5530239.html


### **onMeasure返回的两个参数都有什么信息**

* 两个参数的意义: 父控件对View的宽高约束
* http://blog.csdn.net/xmxkf/article/details/51490283


### **gilde怎么实现圆角图**

```
/**
 * Glide 圆角 Transform
 */

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

 ActivityManagerService -> 
 ActivityStackSupervisor -> 
 ActivityStack -> 
 ActivityStackSupervisor -> 
 ApplicationThread 

* **链接**
    * http://blog.csdn.net/feiduclear_up/article/details/49201357
    * http://blog.csdn.net/xmxkf/article/details/52452218

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

* http://www.cnblogs.com/smyhvae/p/4003922.html

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

### **Glide4.0源码解析**
* **链接**
    * [Android 图片加载框架Glide4.0源码完全解析 一](http://www.cnblogs.com/guanmanman/p/7008259.html)
    * [Android 图片加载框架Glide4.0源码完全解析 二](http://www.cnblogs.com/guanmanman/p/7040942.html)


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