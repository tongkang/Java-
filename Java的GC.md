## Java的GC

#### 1、内存管理的两个流派

- 手动内存管理（C/C++）

- 自动内存管理（几乎所有的现代编程语言）

  - 引用计数（Reference Counting）：只要一个对象没有人引用他，就清除，

    但是有个问题：会出现循环引用

  - 标记清除（Mark and Sweep）：首先标记一个对象不可能是垃圾的，然后在这个对象的调用链上的对象都不是垃圾，除此之外的都是垃圾。



#### 2、内存的碎片化问题

- 长期工作的内存会出现碎片化
  - 总可用空间仍然足够，但是无法分配给对象
- 垃圾回收=回收可用空间+压缩内存



#### 3、引用类型

- 强引用 Strong Reference：你用到的都是强引用
- 软引用 Soft Reference：内存不够的时候会回收他们
- 弱引用 Weak Reference：GC一碰到就回收它们
- 影子引用 Phantom Reference：只能拿到影子，拿不到引用本身



#### 4、哪些东西是GC Roots

- 活的线程（java.lang.Thread）
- 类的静态成员
- 线程方法栈所引用的对象
- JNI引用的对象
- 分代GC时其他代的对象



#### 5、GC发生了什么

- 第一种：首先会停下所有的工作，然后扫描清除垃圾，最后压紧内存
  - Stop the world（STW）
  - 清除垃圾
  - 压紧内存
- 第二种：拷贝，速度稍快，但是会占用两块内存空间



#### 6、对象的分代假设

- 内存分代
  - 年轻代（Young Generation）
  - 老年代（Old Generation/Tenured）
  - 永久代（Permanent Generation）

1. ##### 年轻代：

   - Eden（伊甸园）
     - 新建的对象都在Eden中，大多数对象都活不过Eden
     - 可以分为多个Thread Local Allocation Buffer
   - Survivor（S0/S1或者from/to）
     - Young GC发生时，整个年轻代进入其中一个Survivor区
     - 足够老的对象被提升至老年代（Tenured）

2. ##### 老年代

   - 老年代相对较大
   - 存储足够年老的对象
   - 发生GC的频率较低
     - 清除垃圾
     - 压紧内存

3. ##### 永久代/元空间

   - Java8之前：永久代
     - 是堆的一部分
     - 存储类数据、字符串常量
     - OOM：Permgen space（一般不会出现OOM，如果出现会是下面的情况）
       - 自定义ClassLoader
       - 动态字节码生成
   - Java8+：元空间
     - 不是堆的一部分（-Xmx）
     - 除非特定指定，否则没有上限
     - -XX:MaxMetaspaceSize/OOM:metaspace



#### 7、GC的种类

- Minor GC/Young GC
  - 总是发生在Eden满的时候
  - 会发生STW（Stop the World）
- Major GC vs Full GC
  - 这两个概念并没有明确的定义，这里阐述一般理解
  - Major GC清除老年代
  - Full GC清除整个堆