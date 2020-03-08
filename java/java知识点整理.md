# java学习知识点整理

## 1、java类库

### 泛型之-协变&逆变

#### 协变

```java
    List<? extends Food> foodList = new ArrayList<>();
    List<Apple> appleList = new ArrayList<>();
    foodList = appleList; // ok 协变
    
    foodList.add(new Beef()); // 错误 不能执行添加null 以外的操作
    foodList.add(new Food());// 错误，同上，
    foodlist.add(new Apple()); // 错误，同上
    
    Food food = foodList.get(index); //ok, 把子类引用赋值给父类显然是可以的
```

<？ extends Food> 指明了上界，即表示了集合中存放的对象是 Food 或者Food 的子类，因此foodList 就表示了一个存放 Food 或者Food 的子类的集合，appleList 集合就是一个存放Food 的子类 Apple的集合， 因此可以把 appleList 赋值给 foodList,但是不能对foodList 添加除null 以外的任何对象。为什么不能添加呢？其实很简单，如果可以允许的话，那么foodList 就可以添加Food 或者Food 的子类型，那么上面代码中在foodList = appleList; 赋值后，执行foodList.add(new Beef()); 操作就会导致给 appleList 里面添加了一个Beef 对象，显然这样是不对的。

#### 逆变

```java
    List<? super Fruit> fruitList = new ArrayList<>();
    List<Food> foodList = new ArrayList<>();
    fruitList = foodList; // ok 逆变
    
    foodList.add(new Meat()); 
    
    fruitList.add(new Apple()); // ok，只能添加 Fruit 或者 其子类
    fruitList.add(new Food());// error, 只能添加 Fruit 或者 其子类
    
    Fruit fruit = fruitList.get(0); // error，get出来的元素是Object类型
    Object obj = fruitList.get(0)；// ok 
```


通过<? super Fruit> 指明了下界，即表示了集合中存放的对象只能是 Fruit 或者其父类，因此fruitList 表示了一个存放Fruit 或者其父类的集合，那么就可以把一个Fruit 父类Food 的集合foodList 赋值给 fruitList(即：fruitlist = foodList)，这儿可能有的人就有点疑问了？<? super Fruit> 指明了集合存放的对象是Fruit 或者其父类,那为什么往集合中添加Fruit的父类元素不行呢（fruitList.add(new Food())）？同样我们用反正法来说明，假如可以向<? super E> 集合中添加 E 的父类，那么就可以向fruitList 添加Fruit 的父类（Food or Object)，在本例中，我们把foodList 赋值给了fruitList(fruitList = foodList),既然可以添加父元素，那么我们执行fruitList.add(new Food) 就相当于给 foodList集合里面添加了一个Food元素，这看起来没有什么问题，因为foodList 本来就是用来装Food 元素的，但是如果我们添加的Fruit 父类是Object 对象（或者一个Fruit 实现的和Food 无关的接口，又或者Food继承的一个其他类B,那么B也是 Fruit 的父类），那么就意味着给foodList 里面添加了和Food 无关的类，这样显然是不行的。那么为什么往fruitList 里面添加Fruit或者其子类 元素是可以的，因为fruitList 表示的就是一个存放Fruit 或者其父类的集合，所以这个集合肯定就能装Fruit 或者其子类，比如上面把foodList 赋值给fruitList 后，往fruitList 里面添加 Apple 就相当于往foodList 里面添加Apple,显然是可以的，对于get 操作时为什么读取出来的是Object ，是因为 fruitList 集合表示的存放Fruit 或者其父类的集合，而Fruit 的父类可能有很多，在本例中由于我们知道存的是Food（fruitList = foodList）, 但是在实际中，谁知道在运行时到底存的是哪一个呢？因此为了安全全部定义为Object 类型。因此得出结论：

通过<? super E> 支持逆变，能往集合中添加E 或者E子类的对象。不能添加E的任何父类对象，读取时的对象为Object 类型。


【参考】Java 泛型之-协变&逆变

https://blog.csdn.net/Just_keep/article/details/79482365

## 11、网络编程

### URL&URI

URL是一种URI

URL与URI的区别和联系

https://www.cnblogs.com/polary/p/11795013.html

## 12 JDK调试定位能力

### 12.1 常用JDK命令行工具

这些命令在 JDK 安装目录下的 bin 目录下：

- jps (JVM Process Status）: 类似 UNIX 的 ps 命令。用户查看所有 Java 进程的启动类、传入参数和 Java 虚拟机参数等信息；

- jstat（ JVM Statistics Monitoring Tool）: 用于收集 HotSpot 虚拟机各方面的运行数据;

- jinfo (Configuration Info for Java) : Configuration Info forJava,显示虚拟机配置信息;

- jmap (Memory Map for Java) :生成堆转储快照;

- jhat (JVM Heap Dump Browser ) : 用于分析 heapdump 文件，它会建立一个 HTTP/HTML 服务器，让用户可以在浏览器上查看分析结果;

- jstack (Stack Trace for Java):生成虚拟机当前时刻的线程快照，线程快照就是当前虚拟机内每一条线程正在执行的方法堆栈的集合。

【搞定Jvm面试】 JDK监控和故障处理工具揭秘

https://blog.csdn.net/qq_34337272/article/details/103644552

### 12.2 Jvisualvm
Java VisualVM是一个工具，它提供有关在Java虚拟机上运行的代码的信息。该jvisualvm工具提供了JDK 6，JDK 7和JDK 8。
Java VisualVM不再与JDK捆绑在一起，但您可以从[VisualVM开源项目站点](https://visualvm.github.io/)获取它。

#### 12.2.1 下载安装

从VisualVM开源项目站点(https://visualvm.github.io/)获取最新版本。

手动执行visualvm.exe或者安装idea插件启动visualvm。默认可以查看当前机器的所有java程序运行情况。

![](https://github.com/KaoGu/ResourceRepo/blob/master/java/jdk/visualvm/windows.png?raw=true)


Jvisualvm功能介绍

https://www.cnblogs.com/mzq123/p/11166640.html

jdk自带监控程序jvisualvm的使用

https://blog.csdn.net/u012550080/article/details/81605189

