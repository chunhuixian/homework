# Java基础

##### 1、List 和 Set 的区别

List可以重复，Set不可以重复，set最多可以存放一个null值

##### 2、HashSet 是如何保证不重复的

HashSet 元素乱序

LinkedHashSet保证元素添加的顺序

TreeSet输出按照自然排序

##### 3、HashMap 是线程安全的吗，为什么不是线程安全的（最好画图说明多线程环境下不安全）?

不安全

hashmap的数组长度达到size的75%的时候会进行扩容，扩容的时候调用put()方法，put方法不是线程同步的，当两个key的hash值相同的时候，两个线程同时计算出相同的位置put的时候会丢失一个key。

另外jdk1.8之前，使用头插法会导致环形链表



总结：首先HashMap是**线程不安全**的，其主要体现：

\#1.在jdk1.7中，在多线程环境下，扩容时会造成环形链或数据丢失。

\#2.在jdk1.8中，在多线程环境下，会发生数据覆盖的情况。



###### 3.1补充：头插法和尾插法：



##### 4、HashMap 的扩容过程

如果hashmap的数组长度已经使用了75%就会引起扩容，会新申请一个长度为原来两倍的桶数组，然后将原数组的元素重新映射到新的数组中，原有数据的引用会逐个被置为null。就是在resize()扩容的时候会造成线程不安全。另外当一个新节点想要插入hashmap的链表时，在jdk1.8之前的版本是插在头部，在1.8后是插在尾部



##### 5、HashMap 1.7 与 1.8 的 区别，说明 1.8 做了哪些优化，如何优化的？

https://blog.csdn.net/qq_36520235/article/details/82417949

##### 6、final finally finalize

**final**：修饰符

​	A).如果一个类被声明为final，就意味着它不能再派生出新的子类，不能作为父类被继承。因此，一个类不能同时被声明为abstract抽象类的和final的类。

​	B).如果将变量或者方法声明为final，可以保证它们在使用中不被改变.

　　1)被声明为final的变量必须在声明时给定初值，而在以后的引用中只能读取，不可修改。 
　　2)被声明final的方法只能使用，不能重载。

**finally**：异常处理机制，必须执行

**finalize**：使用finalize()方法在垃圾收集器将对象从内存中清除出去前，做必要的清理工作

##### 7、强引用 、软引用、 弱引用、虚引用

##### 8、Java反射

##### 9★、Arrays.sort 实现原理和 Collection 实现原理

array.sort：先取出前三个，对前三个进行排序。从第四个进行取值，与前三个值比较，进行二分插入

```java
Object[] a = list.toArray();
Arrays.sort(a);
ListIterator<T> i = list.listIterator();
for (int j=0; j<a.length; j++) {
	i.next();
	i.set((T)a[j]);
}
```

总结：Collections.sort方法底层就是调用的array.sort方法，



##### 10、LinkedHashMap的应用

11、cloneable接口实现原理

12、异常分类以及处理机制

13、wait和sleep的区别

##### 11、cloneable接口实现原理

##### 12、异常分类以及处理机制

##### 13、wait和sleep的区别

##### 14、数组在内存中如何分配

​	int[] arr=new int[3];
​	int[] newArr=arr;
​	

	栈存储局部变量（在方法定义中或方法声明上的变量），所以int[] arr 初始化数组地址放在栈中；
	new出的变量放在堆中，所以new int【3】在堆中
	一个new出来的东西都有地址值（系统随机分配），所以new int【3】的地址值为0x001;
	把0x001赋给arr，在栈中的数组通过地址值找到堆中相应的地址。用数组名和编号的配合就可以找 到数组中指定编号的元素，这种方法就叫做索引。
	给数组中的每个元素赋值，把原先的默认值干掉。







# Java 并发

##### 1、synchronized 的实现原理以及锁优化？

##### 2、volatile 的实现原理？

##### 3、Java 的信号灯？

单个信号量的Semaphore对象可以实现互斥锁的功能，并且可以是由一个线程获得了“锁”，再由另一个线程释放“锁”，这可应用于死锁恢复的一些场合。

##### 4、synchronized 在静态方法和普通方法的区别？

synchronized修饰加非static的方法，锁是加在单个对象上，不同的对象没有竞争关系；
synchronized修饰加了static的方法，锁是加载类上，这个类所有的对象竞争一把锁。


5、怎么实现所有线程在等待某个事件的发生才会去执行？

方案一：读写锁
　　刚开始主线程先获取写锁，然后所有子线程获取读锁，然后等事件发生时主线程释放写锁；

方案二：CountDownLatch
　　CountDownLatch初始值设为1，所有子线程调用await方法等待，等事件发生时调用countDown方法计数减为0；

方案三：Semaphore
　　Semaphore初始值设为N，刚开始主线程先调用acquire(N)申请N个信号量，其它线程调用acquire()阻塞等待，等事件发生时同时主线程释放N个信号量；

##### 6、CAS？CAS 有什么缺陷，如何解决？

CAS,compare and swap - 比较并交换。
https://www.cnblogs.com/upcwanghaibo/p/8626802.html

##### 7、synchronized 和 lock 有什么区别？

https://www.jianshu.com/p/b343a9637f95

Synchronized是关键字，内置语言实现，Lock是接口。
Synchronized在线程发生异常时会自动释放锁，因此不会发生异常死锁。Lock异常时不会自动释放锁，所以需要在finally中实现释放锁。
Lock是可以中断锁，Synchronized是非中断锁，必须等待线程执行完成释放锁。
Lock可以使用读锁提高多线程读效率。

##### 8、Hashtable 是怎么加锁的 ？

相关方法上加synchronized关键字

##### 9、HashMap 的并发问题？

https://www.jianshu.com/p/1e9cf0ac07f4

##### 10、HashMap的长度为什么是2的次幂

indexFor()中h & (length - 1)   计算hash值，减少hash碰撞，

##### 10、ConcurrenHashMap 介绍？1.8 中为什么要用红黑树？

##### 11、AQS

AbstractQueuedSynchronizer（AQS - 抽象的队列式的同步器 ）定义了一套多线程访问共享资源的同步器框架，许多同步类实现都依赖于它，如常用的ReentrantLock/Semaphore/CountDownLatch...

流程：

1）调用自定义同步器的tryAcquire()尝试直接去获取资源，如果成功则直接返回；
2）没成功，则addWaiter()将该线程加入等待队列的尾部，并标记为独占模式；
3）acquireQueued()使线程在等待队列中休息，有机会时（轮到自己，会被unpark()）会去尝试获取资源。获取到资源后才返回。如果在整个等待过程中被中断过，则返回true，否则返回false。
4）如果线程在等待过程中被中断过，它是不响应的。只是获取资源后才再进行自我中断selfInterrupt()，将中断补上。


至此，acquire()的流程终于算是告一段落了。这也就是ReentrantLock.lock()的流程，不信你去看其lock()源码吧，整个函数就是一条acquire(1)



##### 12、如何检测死锁？怎么预防死锁？

##### 13、Java 内存模型？

Java内存模型是围绕着并发编程中

https://www.cnblogs.com/yuxiang1/p/10875619.html

原子性、
可见性（　　除了volatile关键字能实现可见性之外，还有synchronized,Lock，final也是可以的。）、
有序性这三个特征来建立

##### 14、如何保证多线程下 i++ 结果正确？

1）compareAndSet
2）加锁
3）synchronized

##### 15、线程池的种类，区别和使用场景？

##### 16、分析线程池的实现原理和线程的调度过程？

##### 17、线程池如何调优，最大数目如何确认？

##### 18、ThreadLocal原理，用的时候需要注意什么？

##### 19、CountDownLatch 和 CyclicBarrier 的用法，以及相互之间的差别?

##### 20、LockSupport工具

##### 21、Condition接口及其实现原理

##### 22、Fork/Join框架的理解

##### 23、分段锁的原理,锁力度减小的思考

##### 24、八种阻塞队列以及各个阻塞队列的特性





# Spring

##### 1、BeanFactory 和 FactoryBean？

##### 2、Spring IOC 的理解，其初始化过程？

##### 3、BeanFactory 和 ApplicationContext？

##### 4、Spring Bean 的生命周期，如何被管理的？

##### 5、Spring Bean 的加载过程是怎样的？

##### 6、如果要你实现Spring AOP，请问怎么实现？

##### 7、如果要你实现Spring IOC，你会注意哪些问题？

##### 8、Spring 是如何管理事务的，事务管理机制？

##### 9、Spring 的不同事务传播行为有哪些，干什么用的？

##### 10、Spring 中用到了那些设计模式？

##### 11、Spring MVC 的工作原理？

##### 12、Spring 循环注入的原理？

##### 13、Spring AOP的理解，各个术语，他们是怎么相互工作的？

##### 14、Spring 如何保证 Controller 并发的安全？

https://www.cnblogs.com/tiancai/p/9627109.html

1）避免定义实例变量
2）在controller中使用threadlocal变量
3）@scope（"prototype"）或在配置文件中声明scope=prototype，但是效率会降低





# Netty

##### 1、BIO、NIO和AIO

##### 2、Netty 的各大组件

##### 3、Netty的线程模型

##### 4、TCP 粘包/拆包的原因及解决方法

##### 5、了解哪几种序列化协议？包括使用场景和如何去选择

##### 6、Netty的零拷贝实现

##### 7、Netty的高性能表现在哪些方面







# 分布式相关

##### 1、Dubbo的底层实现原理和机制

##### 2、描述一个服务从发布到被消费的详细过程

##### 3、分布式系统怎么做服务治理

##### 4、接口的幂等性的概念

##### 5、消息中间件如何解决消息丢失问题

##### 6、Dubbo的服务请求失败怎么处理

##### 7、重连机制会不会造成错误

##### 8、对分布式事务的理解

##### 9、如何实现负载均衡，有哪些算法可以实现？

##### 10、Zookeeper的用途，选举的原理是什么？

##### 11、数据的垂直拆分水平拆分。

##### 12、zookeeper原理和适用场景

##### 13、zookeeper watch机制

##### 14、redis/zk节点宕机如何处理

##### 15、分布式集群下如何做到唯一序列号

##### 16、如何做一个分布式锁

##### 17、用过哪些MQ，怎么用的，和其他mq比较有什么优缺点，MQ的连接是线程安全的吗

##### 18、MQ系统的数据如何保证不丢失

##### 19、列举出你能想到的数据库分库分表策略；分库分表后，如何解决全表查询的问题

##### 20、zookeeper的选举策略

##### 21、全局ID

# 数据库

##### 1、mysql分页有什么优化

##### 2、悲观锁、乐观锁

##### 3、组合索引，最左原则

##### 4、mysql 的表锁、行锁

##### 5、mysql 性能优化

##### 6、mysql的索引分类：B+，hash；什么情况用什么索引

##### 7、事务的特性和隔离级别

# 缓存

##### 1、Redis用过哪些数据数据，以及Redis底层怎么实现

##### 2、Redis缓存穿透，缓存雪崩

##### 3、如何使用Redis来实现分布式锁

##### 4、Redis的并发竞争问题如何解决

##### 5、Redis持久化的几种方式，优缺点是什么，怎么实现的

##### 6、Redis的缓存失效策略

##### 7、Redis集群，高可用，原理

##### 8、Redis缓存分片

##### 9、Redis的数据淘汰策略

# JVM

##### 1、详细jvm内存模型

##### 2、讲讲什么情况下回出现内存溢出，内存泄漏？

##### 3、说说Java线程栈

##### 4、JVM 年轻代到年老代的晋升过程的判断条件是什么呢？

##### 5、JVM 出现 fullGC 很频繁，怎么去线上排查问题？

##### 6、类加载为什么要使用双亲委派模式，有没有什么场景是打破了这个模式？

##### 7、类的实例化顺序

##### 8、JVM垃圾回收机制，何时触发MinorGC等操作

##### 9、JVM 中一次完整的 GC 流程（从 ygc 到 fgc）是怎样的

##### 10、各种回收器，各自优缺点，重点CMS、G1

##### 11、各种回收算法

##### 12、OOM错误，stackoverflow错误，permgen space错误