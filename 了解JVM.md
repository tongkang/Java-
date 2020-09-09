## 了解JVM

#### 一、JLS与JVMS

- Java语言规范 Java Language Specification
  - 定义Java语言的语法
- Java虚拟机规范 Java Virtual Machine Specification
  - 定义字节码如何在JVM中执行



#### 二、JVM的运行时内存区域

- 堆
  - 所有的**对象**都在堆上分配
  - 你只能操纵对象的**引用**，或者地址
  - 你只能操纵对象的诞生，但无法控制它的死亡

- 方法栈
  - 线程私有
  - 每当一个方法调用发生，方法栈就入栈一个栈帧
  - 每当方法退出（返回或者异常），栈帧就被弹出
  - 局部变量在栈帧中分配
  - 当栈深度过大时候抛出StackOverflow
- 本地方法栈
  - 基本上接触不到
- 方法区
  - 被整个虚拟机所共享的Class信息等
  - 运行时常量池
    - 　String.intern()是一个native方法，它的作用是：如果字符串常量池中已经包含了一个等于此String对象的字符串，则返回代表池中这个字符串的String对象；否则，将此String对象包含的字符串添加到常量池中，并返回此String对象的引用。
    - 如果调用`String.equals()`，如果是相同的对象就直接返回了，增加性能，否则就要进入下面的循环比较
  - Java7之前叫：永久代（PermGen）
    - OutOfMemory：perm gen（这个是放在堆中的，会和堆抢占空间）
    - Java8之后：元空间（Metaspace）
      - 元空间和Native共享内存



#### 三、JVM是如何实现平台无关性的？

- 字节码（IR intermediate representation）
  - 一种基于栈的（操作数栈）、与平台无关的计算模型
  - 可以由多种方式生成：
    - JVM上的编程语言
    - 直接生成字节码



#### 四、双亲委派加载模型

- 首先从下往上找，Application ClassLoader-->Extension ClassLoader---->Bootstrap ClassLoader，返回从上往下返回