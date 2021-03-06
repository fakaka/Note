# Java 高级
---

## 泛型
泛型，即“参数化类型”。一提到参数，最熟悉的就是定义方法时有形参，然后调用此方法时传递实参。那么参数化类型怎么理解呢？顾名思义，就是将类型由原来的具体的类型参数化，类似于方法中的变量参数，此时类型也定义成参数形式（可以称之为类型形参），然后在使用/调用时传入具体的类型（类型实参）。

ava 语言中引入泛型是一个较大的功能增强。不仅语言、类型系统和编译器有了较大的变化，以支持泛型，而且类库也进行了大翻修，所以许多重要的类，比如集合框架，都已经成为泛型化的了。这带来了很多好处：

类型安全。 泛型的主要目标是提高 Java 程序的类型安全。通过知道使用泛型定义的变量的类型限制，编译器可以在一个高得多的程度上验证类型假设。没有泛型，这些假设就只存在于程序员的头脑中（或者如果幸运的话，还存在于代码注释中）。

Java 程序中的一种流行技术是定义这样的集合，即它的元素或键是公共类型的，比如“String 列表”或者“String 到 String 的映射”。通过在变量声明中捕获这一附加的类型信息，泛型允许编译器实施这些附加的类型约束。类型错误现在就可以在编译时被捕获了，而不是在运行时当作 ClassCastException 展示出来。将类型检查从运行时挪到编译时有助于您更容易找到错误，并可提高程序的可靠性。

消除强制类型转换。 泛型的一个附带好处是，消除源代码中的许多强制类型转换。这使得代码更加可读，并且减少了出错机会。

泛型中的通配符：可以解决当具体类型不确定的时候，这个通配符就是 ?  ；当操作类型时，不需要使用类型的具体功能时，只使用Object类中的功能。那么可以用 ? 通配符来表未知类型。

泛型限定：
上限：？extends E：可以接收E类型或者E的子类型对象。
下限：？super E：可以接收E类型或者E的父类型对象。

上限什么时候用：往集合中添加元素时，既可以添加E类型对象，又可以添加E的子类型对象。为什么？因为取的时候，E类型既可以接收E类对象，又可以接收E的子类型对象。

下限什么时候用：当从集合中获取元素进行操作的时候，可以用当前元素的类型接收，也可以用当前元素的父类型接收。


## 反射
反射机制是在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法；对于任意一个对象，都能够调用它的任意一个方法和属性；这种动态获取的信息以及动态调用对象的方法的功能称为java语言的反射机制。

反射机制主要提供了以下功能： 

在运行时判断任意一个对象所属的类；

在运行时构造任意一个类的对象；

在运行时判断任意一个类所具有的成员变量和方法；

在运行时调用任意一个对象的方法；

生成动态代理。

### 反射的常见用途
* Junit的注解
* 各种框架的注解

## 枚举
enum 不能使用 extends 关键字继承其他类，因为 enum 已经继承了 java.lang.Enum 。

枚举是类型安全的，不会出现取值范围错误的问题

``` java
public enum Color {  
    RED("红色", 1), GREEN("绿色", 2), BLANK("白色", 3), YELLO("黄色", 4);  
    // 成员变量  
    private String name;  
    private int index;  
    // 构造方法  
    private Color(String name, int index) {  
        this.name = name;  
        this.index = index;  
    }  
    // 普通方法  
    public static String getName(int index) {  
        for (Color c : Color.values()) {  
            if (c.getIndex() == index) {  
                return c.name;  
            }  
        }  
        return null;  
    }  
    // get set 方法  
    /** 
    *
    */
} 
```


## JVM
### Java堆和栈的区别
　　Java把内存划分成两种：一种是栈内存，一种是堆内存。
　　在函数中定义的一些基本类型的变量和对象的引用变量都在函数的栈内存中分配。当在一段代码块定义一个变量时，Java就在栈中为这个变量分配内存空间，当超过变量的作用域后，Java会自动释放掉为该变量所分配的内存空间，该内存空间可以立即被另作他用。
　　堆内存用来存放由new创建的对象和数组。在堆中分配的内存，由Java虚拟机的自动垃圾回收器来管理。在堆中产生了一个数组或对象后，还可以在栈中定义一个特殊的变量，让栈中这个变量的取值等于数组或对象在堆内存中的首地址，在栈中的这个特殊的变量就变成了数组或者对象的引用变量，以后就可以在程序中使用栈内存中的引用变量来访问堆中的数组或者对象，引用变量相当于为数组或者对象起的一个别名，或者代号。
　　引用变量是普通变量，定义时在栈中分配内存，引用变量在程序运行到作用域外释放。而数组＆对象本身在堆中分配，即使程序运行到使用new产生数组和对象的语句所在地代码块之外，数组和对象本身占用的堆内存也不会被释放，数组和对象在没有引用变量指向它的时候，才变成垃圾，不能再被使用，但是仍然占着内存，在随后的一个不确定的时间被垃圾回收器释放掉。这个也是java比较占内存的主要原因。但是在写程序的时候，可以人为的控制。

### Java类加载机制
　　JVM将类加载过程划分为三个步骤：装载、链接和初始化。
装载（Load）：装载过程负责找到二进制字节码并加载至JVM中，JVM通过类的全限定名（com.bluedavy. HelloWorld）及类加载器（ClassLoaderA实例）完成类的加载；
链接（Link）：链接过程负责对二进制字节码的格式进行校验、初始化装载类中的静态变量及解析类中调用的接口、类；
初始化（Initialize）：执行类中的静态初始化代码、构造器代码及静态属性的初始化。


## NIO
### NIO基础概念

### Channel
通道是双向的，通过一个Channel既可以进行读，也可以进行写
Stream只能进行单向操作，通过一个Stream只能进行读或者写

以下是常用的几种通道：

* FileChannel
* SocketChanel
* ServerSocketChannel
* DatagramChannel

通过使用FileChannel可以从文件读或者向文件写入数据；通过SocketChannel，以TCP来向网络连接的两端读写数据；通过ServerSocketChanel能够监听客户端发起的TCP连接，并为每个TCP连接创建一个新的SocketChannel来进行数据读写；通过DatagramChannel，以UDP协议来向网络连接的两端读写数据。

　　下面给出通过FileChannel来向文件中写入数据的一个例子：
``` java
public class Test {
    public static void main(String[] args) throws IOException  {
        File file = new File("data.txt");
        FileOutputStream outputStream = new FileOutputStream(file);
        FileChannel channel = outputStream.getChannel();
        ByteBuffer buffer = ByteBuffer.allocate(1024);
        String string = "java nio";
        buffer.put(string.getBytes());
        buffer.flip();     //此处必须要调用buffer的flip方法
        channel.write(buffer);
        channel.close();
        outputStream.close();
    }  
}
```
　　通过上面的程序会向工程目录下的data.txt文件写入字符串"java nio"，注意在调用channel的write方法之前必须调用buffer的flip方法，否则无法正确写入内容，至于具体原因将在下篇博文中具体讲述Buffer的用法时阐述。

### Buffer
　　Buffer，故名思意，缓冲区，实际上是一个容器，是一个连续数组。Channel提供从文件、网络读取数据的渠道，但是读取或写入的数据都必须经由Buffer。具体看下面这张图就理解了：

　　上面的图描述了从一个客户端向服务端发送数据，然后服务端接收数据的过程。客户端发送数据时，必须先将数据存入Buffer中，然后将Buffer中的内容写入通道。服务端这边接收数据必须通过Channel将数据读入到Buffer中，然后再从Buffer中取出数据来处理。

　　在NIO中，Buffer是一个顶层父类，它是一个抽象类，常用的Buffer的子类有：
* ByteBuffer
* IntBuffer
* CharBuffer
* LongBuffer
* DoubleBuffer
* FloatBuffer
* ShortBuffer

如果是对于文件读写，上面几种Buffer都可能会用到。但是对于网络读写来说，用的最多的是ByteBuffer。

关于Buffer的具体使用以及它的limit、posiion和capacity这几个属性的理解在下一篇文章中讲述。

### Selector

　　Selector类是NIO的核心类，Selector能够检测多个注册的通道上是否有事件发生，如果有事件发生，便获取事件然后针对每个事件进行相应的响应处理。这样一来，只是用一个单线程就可以管理多个通道，也就是管理多个连接。这样使得只有在连接真正有读写事件发生时，才会调用函数来进行读写，就大大地减少了系统开销，并且不必为每个连接都创建一个线程，不用去维护多个线程，并且避免了多线程之间的上下文切换导致的开销。

　　与Selector有关的一个关键类是SelectionKey，一个SelectionKey表示一个到达的事件，这2个类构成了服务端处理业务的关键逻辑。

## NIO的异步I/O


## Java8的新特性

### Lambda表达式

``` java 
Arrays.asList( "a", "b", "d" ).forEach( e -> 
    System.out.println(e) 
);
```
``` java
Arrays.asList( "a", "b", "d" ).forEach( e -> {
    System.out.print(e);
    System.out.print(e);
});
```

### 接口的默认方法与静态方法
### 方法引用
### Stream
### Date/Time API (JSR 310)
### Base64
Base64
在Java 8中，Base64编码已经成为Java类库的标准。它的使用十分简单，下面让我们看一个例子：
``` java
import java.nio.charset.StandardCharsets;
import java.util.Base64;

public class Base64s {
public static void main(String[] args) {
    final String text = "Base64 finally in Java 8!";

    final String encoded = Base64.getEncoder().encodeToString(text.getBytes(StandardCharsets.UTF_8));
    System.out.println(encoded);

    final String decoded = new String(Base64.getDecoder().decode(encoded), StandardCharsets.UTF_8);
    System.out.println(decoded);
}
}
```
程序在控制台上输出了编码后的字符与解码后的字符：

    QmFzZTY0IGZpbmFsbHkgaW4gSmF2YSA4IQ==
    Base64 finally in Java 8!
    
Base64类同时还提供了对URL、MIME友好的编码器与解码器（Base64.getUrlEncoder() / Base64.getUrlDecoder(), Base64.getMimeEncoder() / Base64.getMimeDecoder()）。


## 描述一下JVM 加载class文件的原理机制?
答：JVM 中类的装载是由类加载器（ClassLoader） 和它的子类来实现的，Java中的类加载器是一个重要的Java 运行时系统组件，它负责在运行时查找和装入类文件中的类。
补充：
1.由于Java的跨平台性，经过编译的Java源程序并不是一个可执行程序，而是一个或多个类文件。当Java程序需要使用某个类时，JVM会确保这个类已经被加载、连接(验证、准备和解析)和初始化。类的加载是指把类的.class文件中的数据读入到内存中，通常是创建一个字节数组读入.class文件，然后产生与所加载类对应的Class对象。加载完成后，Class对象还不完整，所以此时的类还不可用。当类被加载后就进入连接阶段，这一阶段包括验证、准备(为静态变量分配内存并设置默认的初始值)和解析(将符号引用替换为直接引用)三个步骤。最后JVM对类进行初始化，包括：1如果类存在直接的父类并且这个类还没有被初始化，那么就先初始化父类；2如果类中存在初始化语句，就依次执行这些初始化语句。
2.类的加载是由类加载器完成的，类加载器包括：根加载器（BootStrap）、扩展加载器（Extension）、系统加载器（System）和用户自定义类加载器（java.lang.ClassLoader的子类）。从JDK 1.2开始，类加载过程采取了父亲委托机制(PDM)。PDM更好的保证了Java平台的安全性，在该机制中，JVM自带的Bootstrap是根加载器，其他的加载器都有且仅有一个父类加载器。类的加载首先请求父类加载器加载，父类加载器无能为力时才由其子类加载器自行加载。JVM不会向Java程序提供对Bootstrap的引用。下面是关于几个类加载器的说明：
a)Bootstrap：一般用本地代码实现，负责加载JVM基础核心类库（rt.jar）；
b)Extension：从java.ext.dirs系统属性所指定的目录中加载类库，它的父加载器是Bootstrap；
c)System：又叫应用类加载器，其父类是Extension。它是应用最广泛的类加载器。它从环境变量classpath或者系统属性java.class.path所指定的目录中记载类，是用户自定义加载器的默认父加载器。

## GC 是什么？为什么要有GC？
答：GC是垃圾收集的意思，内存处理是编程人员容易出现问题的地方，忘记或者错误的内存回收会导致程序或系统的不稳定甚至崩溃，Java提供的GC功能可以自动监测对象是否超过作用域从而达到自动回收内存的目的，Java语言没有提供释放已分配内存的显示操作方法。Java程序员不用担心内存管理，因为垃圾收集器会自动进行管理。要请求垃圾收集，可以调用下面的方法之一：System.gc() 或Runtime.getRuntime().gc() ，但JVM可以屏蔽掉显示的垃圾回收调用。
垃圾回收可以有效的防止内存泄露，有效的使用可以使用的内存。垃圾回收器通常是作为一个单独的低优先级的线程运行，不可预知的情况下对内存堆中已经死亡的或者长时间没有使用的对象进行清除和回收，程序员不能实时的调用垃圾回收器对某个对象或所有对象进行垃圾回收。在Java诞生初期，垃圾回收是Java最大的亮点之一，因为服务器端的编程需要有效的防止内存泄露问题，然而时过境迁，如今Java的垃圾回收机制已经成为被诟病的东西。移动智能终端用户通常觉得iOS的系统比Android系统有更好的用户体验，其中一个深层次的原因就在于Android系统中垃圾回收的不可预知性。
补充：垃圾回收机制有很多种，包括：分代复制垃圾回收、标记垃圾回收、增量垃圾回收等方式。标准的Java进程既有栈又有堆。栈保存了原始型局部变量，堆保存了要创建的对象。Java平台对堆内存回收和再利用的基本算法被称为标记和清除，但是Java对其进行了改进，采用“分代式垃圾收集”。这种方法会跟Java对象的生命周期将堆内存划分为不同的区域，在垃圾收集过程中，可能会将对象移动到不同区域：
•	伊甸园（Eden）：这是对象最初诞生的区域，并且对大多数对象来说，这里是它们唯一存在过的区域。
•	幸存者乐园（Survivor）：从伊甸园幸存下来的对象会被挪到这里。
•	终身颐养园（Tenured）：这是足够老的幸存对象的归宿。年轻代收集（Minor-GC）过程是不会触及这个地方的。当年轻代收集不能把对象放进终身颐养园时，就会触发一次完全收集（Major-GC），这里可能还会牵扯到压缩，以便为大对象腾出足够的空间。
与垃圾回收相关的JVM参数：
•	-Xms / -Xmx --- 堆的初始大小 / 堆的最大大小
•	-Xmn --- 堆中年轻代的大小
•	-XX:-DisableExplicitGC --- 让System.gc()不产生任何作用
•	-XX:+PrintGCDetail --- 打印GC的细节
•	-XX:+PrintGCDateStamps --- 打印GC操作的时间戳

## Unicode
实在看不下去了 出来澄清一些事实
很多人都把Unicode编码挂在嘴边，其实咱们现实生活中遇到的编码基本都是Unicode的
因为Unicode兼容了大多数老版本的编码规范例如 ASCII
Unicode编码定义了这个世界上几乎所有字符（就是你眼睛看到的长那个样子的符号）的数字表示
也就是说Unicode为每个字符发了一张身份证，这张身份证上有一串唯一的数字ID确定了这个字符
在这个纷乱世界上存在的唯一性。Unicode给这串数字ID起了个名字叫［码点］（Code Point）
而很多人说的编码其实是想表达［Unicode转换格式］（即UTF，Unicode Transformation Formats）
有没有觉得眼前一亮豁然开朗？没错 这就是我们看到的UTF-8/UTF-16/UTF-32的前缀来源
这个［Unicode转换格式］的存在是为了解决［码点］在计算机中的二进制表现形式而设计的
毕竟我们的机内表示涉及存储位宽，兼容古老编码格式，码点是数值过大的罕见字符等问题
［码点］经过映射后得到的二进制串的转换格式单位称之为［码元］（Code Unit）。也就是说如果有一种UTF的码点二进制表示有n字节，其码元为8位（1个byte），那么其拥有码元n个。每种UTF的码元都不同，其宽度被作为区分写在了UTF的后缀——这就是UTF-8/UTF-16/UTF-32的由来。UTF-8的码元是8位的，UTF-16的码元是16位的。大部分的编程语言采用16位的码元作为机内表示。这就是我们在各种语言中调用获取一个字符串中character的数量时会出现这么多混乱的原因。事实上我们调用这些方法时取得的不是字符个数，而是码元个数！一旦我们的字符串中包含了位于基本平面之外的码点，那么就会需要更多的码元来表示，这个时候就会出现测试时常见的困惑——为何return的字符数比实际字符数要多？所以实际写代码时要特别注意这个问题。
采取不同的映射方式可以得到不同格式的二进制串，但是他们背后所表示的［码点］永远是一致的就好像你换身份证但是身份证号不变一样。由于平时人们误把［转换格式］也称为［编码］，所以造成今天Unicode／UTF傻傻分不清楚且遣词造句运用混乱的悲桑局面。
Unicode 编码 发展到今天 扩展到了 21 位（从 U+0000 到 U+10FFFF ）。这一点很重要： Unicode 不是 16 位的编码， 它是 21 位的。这 21 位提供了 1,114,112 个码点，其中，只有大概 10% 正在使用，所以还有相当大的扩充空间。
编码空间被分成 17 个平面（plane），每个平面有 65,536 个字符（正好填充2个字节，16位）。0 号平面叫做「基本多文种平面」（BMP, Basic Multilingual Plane ），涵盖了几乎所有你能遇到的字符，除了 emoji（emoji位于1号平面 - -）。其它平面叫做补充平面，大多是空的。
总结一下各种编码格式的特质：

### UTF-32
最清楚明了的一个 UTF 就是 UTF-32 ：它在每个码点上使用整 32 位。32 大于 21，因此每一个 UTF-32 值都可以直接表示对应的码点。尽管简单，UTF-32却几乎从来不在实际中使用，因为每个字符占用 4 字节太浪费空间了。


### UTF-16 以及「代理对」（ Surrogate Pairs ）的概念
UTF-16要常见得多，它是根据有 16 位固定长度的码元（ code units ）定义的。UTF-16 本身是一种长度可变的编码。基本多文种平面（BMP）中的每一个码点都直接与一个码元相映射。鉴于 BMP 几乎囊括了所有常见字符，UTF-16 一般只需要 UTF-32 一半的空间。其它平面里很少使用的码点都是用两个 16 位的码元来编码的，这两个合起来表示一个码点的码元就叫做代理对（ surrogate pair ）。

### UTF-8
UTF-8 使用一到四个字节来编码一个码点。从 0 到 127 的这些码点直接映射成 1 个字节（对于只包含这个范围字符的文本来说，这一点使得 UTF-8 和 ASCII 完全相同）。接下来的 1,920 个码点映射成 2 个字节，在 BMP 里所有剩下的码点需要 3 个字节。Unicode 的其他平面里的码点则需要 4 个字节。UTF-8 是基于 8 位的码元的，因此它并不需要关心字节顺序（不过仍有一些程序会在 UTF-8 文件里加上多余的 BOM）。

有效率的空间使用（仅就西方语言来讲），以及不需要操心字节顺序问题使得 UTF-8 成为存储和交流 Unicode 文本方面的最佳编码。它也已经是文件格式、网络协议以及 Web API 领域里事实上的标准了。
我们的JVM中保存码点是UTF16的转换格式，从char的位宽为16位也可以看得出来。由于绝大部分编码的码点位于基本平面，所以使用16位可以几乎表示所有常用字符。这就是许多语言编译器或运行时都使用UTF16的原因。英文在使用UTF16时也是2字节表示的。当我们想要使用其他平面的字符时，码元超过2个字节，就需要使用代理对在语言中的特定表示方式，譬如‘\\U112233’之类的。
使用UTF8时，常用的Alphabet和Numeric都在前127字节，被有效率地用一个字节表示。而我们的中文由于排在1920个码点之后，所以使用3个字节表示，这方面就比UTF16转换格式耗费更多空间。
最后，不论使用哪种UTF转换格式，都是程序员自己可以选择的一种表达方式而已。我们可以通过Java方便的API进行自如转换。