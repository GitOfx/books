1,ui线程更新
只有创建线程中才能更新UI
onresume
viewRootImpl创建后
checkThread
在子线程中拿到创建view 也可以更新UI

2 startService和bindService的区别
startService   onCreate() ==> onStartCommand();
调用多次startService，onCreate只有第一次会被执行，而onStartCommand会执行多次。
结束服务时，调用stopService，生命周期执行onDestroy方法，并且多次调用stopService时，onDestroy只有第一次会被执行。
bingService  onCreate() ==> onBind();
调用多次bindService，onCreate和onBind也只在第一次会被执行。
调用unbindService结束服务，生命周期执行onDestroy方法，并且unbindService方法只能调用一次，多次调用应用会抛出异常。使用时也要注意调用unbindService一定要确保服务已经开启，否则应用会抛出异常

3 activty 启动模式
    standard  多实例  所任务栈
    singleTask  单实例  清空前面的
    singleTop  重复不会创建  onnewIntent
    singleInstance  单实例  单任务栈

4 activity与fragment通信方式
  activity传数据到fragment bundle
  fragment 调用activity 将activity实现接口传入  接口回调
activity之间通信
Intent Bundle
静态变量
全局变量 及Application
Android系统剪切板
本地化存储方式
Andorid组件
EventBus
本地广播 localBoardcastManager 里面也是通过handler来管理通知
广播
1)第一种不是常驻型广播，也就是说广播跟随程序的生命周期。

2)第二种是常驻型，也就是说当应用程序关闭后，如果有信息广播来，程序也会被系统调用自动运行

5 Android消息机制
https://blog.csdn.net/yanzhenjie1003/article/details/89218745?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_title~default-0.pc_relevant_default&spm=1001.2101.3001.4242.1&utm_relevant_index=3
Android通过Looper、Handler来实现消息循环机制。
Android的消息循环是针对线程的，每个线程都可以有自己的消息队列和消息循环。
Android系统中的Looper负责管理线程的消息队列和消息循环。
通过Looper.myLooper()得到当前线程的Looper对象，
通过Looper.getMainLooper()得到当前进程的主线程的Looper对象。
前面提到，Android的消息队列和消息循环都是针对具体线程的，一个线程可以存在一个消息队列和消息循环，特定线程的消息只能分发给本线程，不能跨线程和跨进程通讯。
但是创建的工作线程默认是没有消息队列和消息循环的，如果想让工作线程具有消息队列和消息循环，

6 view 事件分发
    activity -viewgroup-view
dispatchTouchEvent- onInterceptTouchEvent - onTouchEvent  子类不处理 返回给父view  viewgroup拦截  自己处理
当前控件(子控件，儿子)收到前驱事件(ACTION_MOVE或者ACTION_MOVE)后，它的父控件(老爸)突然插手，截断事件的传递，这时，当前控件就会收到ACTION_CANCEL，
收到此事件后，不管子控件此时返回true或者false，都认为这一个动作已完成，不会再回传到父控件的OnTouchEvent中处理
1、一旦View消费了Down事件，那么后续的Move、Up、Cancel等事件都会交给它处理(ViewGroup没有拦截消费的前提下)
2、即使滑动超出了当前View的范围，它依然能够收到上述事件。
3、若是最终的Up事件不是发生在当前View之上，那么该View不响应单击与长按动作。
4、若是收到Cancel事件，那么View就不会再响应单击与长按动作，并且后续的事件将不会再收到
onTouch() 方法的返回值决定了 onTouchEvent() 方法要不要执行

7 Serializable和Parcelelable的区别
Serializable是属于 Java 自带的，表示一个对象可以转换成可存储或者可传输的状态，序列化后的对象可以在网络上进行传输，也可以存储到本地。
Parcelable 是属于 Android 专用。不过不同于Serializable，Parcelable实现的原理是将一个完整的对象进行分解。而分解后的每一部分都是Intent所支持的数据类型


8 res/raw和assets的相同点：
assets:用于存放需要打包到应用程序的静态文件，以便部署到设备中。与res/raw不同点在于，ASSETS支持任意深度的子目录。这些文件不会生成任何资源ID，必须使用/assets开始（不包含它）的相对路径名。

res:用于存放应用程序的资源（如图标、GUI布局等），将被打包到编译后的Java中。不支持深度子目录

9 view的绘制
performTraversals Measure  Layout  Draw

10 主流框架
    Glide  retrofit rxjava 

11 主流架构
 mvi mvp mvvm 

12 内存泄漏场景 原因 检测方式  
  原因：1 对象可用不可达 2 长周期被短周期引用
  场景 1 异步任务对象未被销毁
       2 数据库 文件资源未回收
       3 匿名内部类 非静态内部类周期超过外部类 hanlder
       4  注册监听未注销
       5 webview未销毁
       6 工具类context用application未销毁
 检测方式 
  1 leackcanary 第三方工具  原理：ReferenceQueue 可以保存被回收过的对象
     监听activtiy的生命周期  在destroy中创建引用队列 如果在队列中就回收过  如果没有 再gc一次 查看队列
  2 profiler工具分析

13 oom问题 方式
https://juejin.cn/post/7074762489736478757
线程数太多   用线程池代替new thread  线程泄漏
打开太多文件
内存不足

14 JVM的内存结构和内存分配
jvm将虚拟机分为5大区域，程序计数器、虚拟机栈、本地方法栈、java堆、方法区；

方法区同 Java 堆一样是被所有线程共享的区间，用于存储已被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码 
堆是Java虚拟机所管理的内存中最大的一块存储区域。堆内存被所有线程共享。主要存放使用new关键字创建的对象。所有对象实例以及数组都要在堆上分配。垃圾收集器就是根据GC算法，收集堆上对象所占用的内存空间（收集的是对象占用的空间而不是对象本身）
VM 中的栈包括 Java 虚拟机栈和本地方法栈，两者的区别就是，Java 虚拟机栈为 JVM 执行 Java 方法服务，本地方法栈则为 JVM 使用到的 Native 方法服务。两者作用是极其相似的，本文主要介绍 Java 虚拟机栈，以下简称栈。

15 aidl IPC  bind原理
  适用跨进程多线程通信
  bind是android系统进程间通信的桥梁  只用copy一次数据
  进程间用户空间独立，通过binder创建缓冲区
  内核空间是公共的，bind通过再内核空间建立映射 
  发送端拷贝数据到内核缓冲区  内核缓冲区与接收端建立映射关系 
  优点  安全 明确的pid
  https://blog.csdn.net/superjunjin/article/details/80181873

16 单例的写法 和注意事项
https://www.cnblogs.com/cielosun/p/6582333.html
https://zhuanlan.zhihu.com/p/150004430

17 anr场景类型
activity input事件在5S内没有处理完成发生了ANR
前台Broadcast：onReceiver在10S内没有处理完成发生ANR。
后台Broadcast：onReceiver在60s内没有处理完成发生ANR
前台Service：onCreate，onStart，onBind等生命周期在20s内没有处理完成发生ANR。
后台Service：onCreate，onStart，onBind等生命周期在200s内没有处理完成发生ANR
ContentProvider 在10S内没有处理完成发生ANR
adb pull /data/anr/traces.txt
注意回调线程

常用数据结构

HashMap 实现了 Map 接口，用于存储键值对，与 Hashtable 不同的是，HashMap 允许 null 元素。是非线程安全的，可以通过 Collections 类获取同步的包装对象
线程安全的高并发哈希表可以使用 ConcurrentHashMap
Hashtable 实现了 Map 接口，用于存储键值对，禁止 null 元素
问题6：ArrayMap 与 HashMap比较？优缺点？应用场景？
比较：
1、ArrayMap采用的数据结构是两个一维数组，而HashMap使用的是一维数组和单链表数据结构
2、ArrayMap默认容量为0，而HashMap默认容量是16
3、ArrayMap没有最大容量的限制，直到报oom，而HashMap最大容量最大是Integer.MAX_VALUE
4、ArrayMap默认每次扩容时原来容量一半的增量；而HashMap默认每次扩容时原来容量0.75倍的增量
优点：
1、相比HashMap内存空间更优，因为比HashMap少了一个实体类进行装饰
2、容量为4或者8时又缓存复用功能
3、扩容比HashMap高效，因为HashMap扩容时相当于重构，需要重新重新计算hash值和移动元素；而ArrayMap扩容时只需拷贝
缺点：
1、数据量大的情况下查询效率比HashMap差
2、存取效率比HashMap低，因为每次存取都需要二分法找到对应的下标
3、没有实现Serializable，不便在Android bundle进行传输

使用场景：
1、数据量小，建议百量级别
2、内存要求高
SparseArray key是int 的时候用

19  SharedPreference
SharedPreference 相关修改使用 apply 方法进行提交会先写入内存，然后异步写入磁盘，commit
方法是直接写入磁盘。如果频繁操作的话 apply 的性能会优于 commit，apply会将最后修改内容写入磁盘。
但是如果希望立刻获取存储操作的结果，并据此做相应的其他操作，应当使用 commit。
 
加班情况 
应用情况  多少个项目
开发人员比例 
上线周期
人员流动 文化
公司规模  福利 活动

