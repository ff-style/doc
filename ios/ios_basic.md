## id
```
 1. id foo1;
    2. NSObject *foo2;
    3. id<NSObject> foo3;
```

简单地申明了指向对象的指针，没有给编译器任何类型信息，因此，编译器不会做类型检查。但也因为是这样，你可以发送任何信息给id类型的对象
因此，id类型是运行时的动态类型，编译器无法知道它的真实类型，即使你发送一个id类型没有的方法，也不会产生编译警告。
通常
如果你不需要任何的类型检查，使用id，它经常作为返回类型，也经常用于申明代理(delegate)类型。因为代理类型通常在运行时，才会检查是否实现了那些方法。
如果
真的需要编译器检查，那你就考虑使用第2种或者第3种


## SEL
```
@property (assign, nonatomic) SEL beginRefreshingAction;

SEL s1 = @selector(test1); // 将test1方法包装成SEL对象　 
SEL s2 = NSSelectorFromString(@"test1"); // 将一个字符串方法转换成为SEL对象 

```
SEL就是对方法的一种包装。包装的SEL类型数据它对应相应的方法地址，找到方法地址就可以调用方法

1.方法的存储位置

在内存中每个类的方法都存储在类对象中
每个方法都有一个与之对应的SEL类型的数据
根据一个SEL数据就可以找到对应的方法地址，进而调用方法
SEL类型的定义:  typedef struct objc_selector *SEL
2.SEL对象的创建



## ^ () {} 
^说明一个块函数，通常后面跟有“()”和“{}”。
（）是块里面需要的参数，｛｝是执行体。
^、()、{}均属于block文档，关于block苹果官方的定义：block对象是C级别的语法和运行时特性。它们和标准C函数很相似，但除了可执行代码外，它们还可能包含了变量自动绑定或内存托管。一个block维护一个状态集（数据），它们可以在执行的时候用来影响程序行为。



## xcode 用颜色
xcode 用颜色来区分 class归属很好用
默认： 绿色代表－project  蓝色代表 other



## @autoreleasepool 
### 意义在于提高 频繁的创建和销毁时候的效率
__autoreleasing
Autorelease实际上是把对release的调用延迟了，对于每一次autorelease，系统只是把对象放入了当前的autorelease pool中，当pool被释放时，pool中所有的对象都会被调用release。



##  MRC ARC
 MRC（Mannul Reference Counting）和ARC(Automatic Reference Counting)，分别对应着手动引用计数和自动引用计数。






##  xib constraint 用法


