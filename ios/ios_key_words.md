# ios关键字整理
例如

```
//@property (nonatomic, weak, readonly) UIScrollView *scrollView;

```

### 1.atomic与nonatomic
atomic：默认是有该属性的，这个属性是为了保证程序在多线程情况，编译器会自动生成一些互斥加锁代码，避免该变量的读写不同步问题。

nonatomic：如果该对象无需考虑多线程的情况，请加入这个属性，这样会让编译器少生成一些互斥加锁代码，可以提高效率。

### 2.readwrite与readonly
readwrite：这个属性是默认的情况，会自动为你生成存取器。
readonly：只生成getter不会有setter方法。
readwrite、readonly这两个属性的真正价值，不是提供成员变量访问接口，而是控制成员变量的访问权限。

### 3.strong与weak
strong：强引用，也是我们通常说的引用，其存亡直接决定了所指向对象的存亡。如果不存在指向一个对象的引用，并且此对象不再显示在列表中，则此对象会被从内存中释放。
weak：弱引用，不决定对象的存亡。即使一个对象被持有无数个弱引用，只要没有强引用指向它，那么还是会被清除。
strong与retain功能相似；weak与assign相似，只是当对象消失后weak会自动把指针变为nil;

### 4.assign、copy、retain
assign：默认类型，setter方法直接赋值，不进行任何retain操作，不改变引用计数。一般用来处理基本数据类型。
retain：释放旧的对象（release），将旧对象的值赋给新对象，再令新对象引用计数为1。我理解为指针的拷贝，拷贝一份原来的指针，释放原来指针指向的对象的内容，再令指针指向新的对象内容。
copy：与retain处理流程一样，先对旧值release，再copy出新的对象，retainCount为1.为了减少对上下文的依赖而引入的机 制。我理解为内容的拷贝，向内存申请一块空间，把原来的对象内容赋给它，令其引用计数为1。对copy属性要特别注意：被定义有copy属性的对象必须要 符合NSCopying协议，必须实现- (id)copyWithZone:(NSZone *)zone方法。
也可以直接使用：
    使用assign: 对基础数据类型 （NSInteger，CGFloat）和C数据类型（int, float, double, char, 等等）
    使用copy： 对NSString
    使用retain： 对其他NSObject和其子类
    
###  5.getter setter
getter：是用来指定get方法的方法名
setter：是用来指定set访求的方法名
在@property的属性中，如果这个属性是一个BOOL值，通常我们可以用getter来定义一个自己喜欢的名字，例如：
@property (nonatomic, assign, getter=isValue) boolean value;
@property (nonatomic, assign, setter=setIsValue) boolean value;


### 区别
1. 
假设你用malloc分配了一块内存，并且把它的地址赋值给了指针a，后来你希望指针b也共享这块内存，于是你又把a赋值给（assign）了b。此时a
和b指向同一块内存，请问当a不再需要这块内存，能否直接释放它？答案是否定的，因为a并不知道b是否还在使用这块内存，如果a释放了，那么b在使用这块
内存的时候会引起程序crash掉。

2. 
了解到1中assign的问题，那么如何解决？最简单的一个方法就是使用引用计数（reference 
counting），还是上面的那个例子，我们给那块内存设一个引用计数，当内存被分配并且赋值给a时，引用计数是1。当把a赋值给b时引用计数增加到 
2。这时如果a不再使用这块内存，它只需要把引用计数减1，表明自己不再拥有这块内存。b不再使用这块内存时也把引用计数减1。当引用计数变为0的时候，
代表该内存不再被任何指针所引用，系统可以把它直接释放掉。

3. 上面两点其实就是assign和retain的区别，assign就是直接赋值，从而可能引起1中的问题，当数据为int, float等原生类型时，可以使用assign。retain就如2中所述，使用了引用计数，retain引起引用计数加1, release引起引用计数减1，当引用计数为0时，dealloc函数被调用，内存被回收。
 
4. copy是在你不希望a和b共享一块内存时会使用到。a和b各自有自己的内存。

5. atomic和nonatomic用来决定编译器生成的getter和setter是否为原子操作。在多线程环境下，原子操作是必要的，否则有可能引起错误的结果。加了atomic，setter函数会变成下面这样：


if (property != newValue) {   
    [property release];   
    property = [newValue retain];   
}

二,深入理解一下(包括autorelease)1. retain之后count加一。alloc之后count就是1，release就会调用dealloc销毁这个对象。
如果 retain，需要release两次。通常在method中把参数赋给成员变量时需要retain。
例如：
ClassA有 setName这个方法：
-(void)setName:(ClassName *) inputName
{
   name = inputName;
   [name retain]; //此处retian，等同于[inputName retain],count等于2
}
调用时：
ClassName *myName = [[ClassName alloc] init];
[classA setName:myName]; //retain count == 2
[myName release]; //retain count==1，在ClassA的dealloc中release name才能真正释放内存。

2. autorelease 更加tricky，而且很容易被它的名字迷惑。我在这里要强调一下：autorelease不是garbage collection，完全不同于Java或者.Net中的GC。
autorelease和作用域没有任何关系！
autorelease 原理：
a.先建立一个autorelease pool
b.对象从这个autorelease pool里面生成。
c.对象生成 之后调用autorelease函数，这个函数的作用仅仅是在autorelease pool中做个标记，让pool记得将来release一下这个对象。
d.程序结束时，pool本身也需要rerlease, 此时pool会把每一个标记为autorelease的对象release一次。如果某个对象此时retain count大于1，这个对象还是没有被销毁。
上面这个例子应该这样写：
ClassName *myName = [[[ClassName alloc] init] autorelease];//标记为autorelease
[classA setName:myName]; //retain count == 2
[myName release]; //retain count==1，注意，在ClassA的dealloc中不能release name，否则release pool时会release这个retain count为0的对象，这是不对的。

记住一点：如果这个对象是你alloc或者new出来的，你就需要调用release。如果使用autorelease，那么仅在发生过retain的时候release一次（让retain count始终为1）。


3 xcode 中的新标记 strong weak

strong 用来修饰强引用的属性；对应以前retain

 

weak 用来修饰弱引用的属性；对应以前的assign



### dispatch_once
