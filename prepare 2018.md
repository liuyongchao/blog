[toc]
# 2 基础篇
## 2.1 基本功
### 面向对象的特征
- 封装
将客观事物封装成抽象的类，隐藏属性， 对外开放访问属性的方法；
- 继承d
根据已存在的类， 建立新的类；可以方便的复用之前的代码， 提高开发效率；
- 多态
父类引用指向子类对象；可以解决项目中紧耦合问题；

### final, finally, finalize 的区别
- final  
修饰变量和方法；  
修饰类表示类不能被继承；  
修饰变量表示变量不能再更改；
修饰的方法只能被使用， 不能被重载；
- finally
finally是异常处理时， 一定会被执行的一个代码块， 一般用来关闭一些连接；
- finalize
finalize是垃圾回收器在回收一个对象之前， 会调用这个对象的finalize()方法;

### int 和 Integer 有什么区别
int是基本类型, Integer是int的包装类型,
Integer的比较不能用 == , 必须用equels

### 重载和重写的区别
- 重载
重载是同一个类中, 方法名相同, 但是参数的数量, 类型不同的两个方法, 构成重载;
- 重写
重写是子类覆写从父类继承的方法; 重写的方法 不能抛更大异常, 限定符的范围不能缩小

### 抽象类和接口有什么区别
- 抽象类
抽象类除了不能实例化对象外,其他和不同java类没有大的区别;  
如果基础方法易改变,且想让子类必须实现一些方法,则可以使用抽象类;
- 接口
接口也不能实例化, 且接口中的方法, 必须被实现接口的类全部实现;

### 说说反射的用途及实现
动态的获取类及其方法和属性,
可以用classForName(String name)方法获取类对象, 也可以使用某个对象的getClass方法;  
- 在RPC框架中,客户端将实现类的名称, 方法名和参数发给服务端,服务端则通过反射获取相应的类和方法,然后执行;
- 在注解的实现中,通过反射获取类的注解;
- 通过方法名获取指定方法并执行;

### 说说自定义注解的场景及实现
通过注解方便的实现一些事情:
- 权限限制  
通过注解来判定制定权限是否有执行该方法的权限;  

注解的实现步骤:
- 实现自定义注解类;
- 反射获取注解, 并做一些事情;

### HTTP 请求的 GET 与 POST 方式的区别
- get是请求数据, post是提交要被处理的数据;
- get请求参数在url中, post是在消息体中;

### session 与 cookie 区别
http是无状态协议, 所以需要利用cookie在session使用户保持状态;
- cookie在客户端,session在服务端;
- 浏览器第一次请求服务端, 服务端会在response header中设置set-cookie,从而设置域下的cookie;
- 再次访问这个域时, 会带上这个cookie;

### session 分布式处理
- nginx 按照ip分配后端;
- session共享,tomcat广播 或者 利用redis进行session共享;

### JDBC 流程
JDBC是SUN公司指定的一系列数据库连接接口规范, 需要各个厂商自己去实现;
一般分为以下步骤:
- 加载驱动;
- 建立连接;
- 发送诉求;
- 获取结果;
- 关闭连接;

### MVC 设计思想
分层的思想,  降低耦合
- controller控制器, 类似导航的作用, 组合不同的模型和视图
- model 模型, 处理业务逻辑;
- view 展示层

### equals 与 == 的区别
equals 时调用对象的equals方法比较, == 是比较两个引用是否指向同一个对象;


## 2.2 集合
### List 和 Set 区别
- list和set都实现了collection接口;
- list允许重复对象, set不允许;
- list有序, 除了treeSet, 其他set无序;
- list可以通过下标遍历, 可以用通过迭代器遍历, 而set只能通过迭代器;

### List 和 Map 区别
- list只存储值, 而map存储一个个的键值对;
- list允许重复对象, map的键不能重复;

### Arraylist 与 LinkedList 区别
- ArrayList底层是一个可以扩充的数组,默认长度10; 
- LinkedList底层是由链表组成的,每个Node节点都有Element e, Node next, Node prev三个成员变量组成;
- ArrayList随机访问快, 添加移除慢;
- LinkedList 随机访问慢, 添加移除快

### ArrayList 与 Vector 区别
- Vector线程安全

### HashMap 和 Hashtable 的区别
- HashTable线程安全, 但实现的不太好,已经被淘汰;

### HashSet 和 HashMap 区别
- HashMap基于散列表的实现,
- HashMap底层由一个Node对象数组组成 , Node[K, V][] table;
- HaseSet底层由一个HashMap实现，元素存放在map的key中。

### HashMap 和 ConcurrentHashMap 的区别
- HashMap非线程安全, ConcurrentHashMap线程安全;

### HashMap 的工作原理及代码实现
- 基于散列表实现,底层由一个Node对象数组组成 , Node[K, V][] table;
- Nod数组的默认初始长度位16，当map中键值对的数量大于阀值的时候，数组resize，容量翻倍。
- 相同hash值的bucket桶的长度小于8时,使用链表来存储元素, 在等于8时,将链表转为红黑树结构,TreeNode<K, V>来存储;
- 根据key确定桶的index    
根据key计算得到hashcode，然后用map的长度length对hashcode取模，即hashcode % length

### ConcurrentHashMap 的工作原理及代码实现
- ConcurrentHashMap是一个Segment数组, 数组默认长度也是16, 不可以扩容, 所以ConcurrentHashMap最大支持16个线程并发写;
- Segment是ConcurrentHashMap中的一个内部类, 继承ReentrantLock;
- Segment内部是由数组+链表组成的;
- put操作, 根据hash值找到对应的segment,获取改segment的独占锁, 然后把数据放入segment;

put操作中的关键操作:
- map初始化的时候会初始化第一个segment[0], 其他segment在插入第一个值的时候初始化, 初始化的时候获取segment[0]的长度和扩容因子;

初始化一个segment的并发问题: 
- 使用unsafe.getObjectVolatile()方法确认获得的是最新的segment对象, 对象为空的情况下初始化segment对象, 使用CAS操作设置segment对象;

- 添加和删除操作都是加了segment的独占锁的,所以没有线程问题;

get过程是没有加锁的, 因为put和remove以及get中的操作都是volatile的, 所以get过程不受影响;




## 2.3 线程
### 创建线程的方式及实现
- 继承Thread
- 实现Runnable
- 实现Callable 可以有返回值

### sleep() 、join（）、yield（）有什么区别
- sleep()使线程暂停一定时间,进入阻塞状态 不释放锁;
- yield()是使线程进入可执行状态;
- join()是等待, 如果在main线程中有t1.join(),则main线程需要等地t1线程执行完才能继续执行; join原理是调用了t1的wait方法;

### 说说 CountDownLatch 原理
CountDownLatch是一个同步工具, 允许一个或多个线程等待其他线程;  
一个线程调用wait方法, 只有指定数量个线程执行countDown方法后, wait的方法后边的代码才能执行;
### 说说 CyclicBarrier 原理
CyclicBarrier是线程等待, 只有当所有线程到齐时, 大家在一起执行;
直到满足数量的线程都执行wait方法后, 所有线程才一起可以继续执行;
### 说说 Semaphore 原理
流量控制器, 有acquire()和release()方法, 指定数量的Semaphore对象后, 只有指定数量的线程可以执行acquire()后边的代码, 超过的线程会阻塞在acquire, 直到有线程release()
### 说说 Exchanger 原理
交换器, 两个线程都到达同步点时, 交换数据;调用exange()方法;
### 说说 CountDownLatch 与 CyclicBarrier 区别
- CountDownLatch相当于等几个人, 但是来个人之后只记下数, 然后这个人就走了,而等人的人只有等经过的人够数了才能走; 
- CyclicBarrier等人, 人来了之后和我一起等, 人到齐了然后大家一起走.

### ThreadLocal 原理分析
ThreadLocal相当于每个线程中都有一个局部变量, 每个线程中都有一个threadMap变量, map的key是 threadLocal变量, value即该线程中, threadLocal变量的值. 
- get方法从当前线程的threadMap中取出值;
- set方法向当前线程的threadMap中set值

### 讲讲线程池的实现原理
要从ThreadPoolExecutor的构造说起, 比较全的一个构造参数有    
从corePoolSize,  maximumPoolSize, keepAliveTime, timeUnit, workQueue, threadFactory, rejecthandler说起;
- keepAliveTime代表, 超过core数量的空闲线程的最大等待时间, 即空闲后最大存活时间;

### 线程池的几种方式与使用场景
可以通过Executors.方式直接获取的线程池有如下几种:
- Executors.newFixedThreadPool()
相当于一个core和max均为指定数量, 最大运行线程为指定数量, 工作队列为无界队列;
- Executors.newCachedThreadPool()
相当于一个core为0,max为int最大值, 工作队列为SynchronousQueue;  
这个队列内部没有存储空间, 在提交任务后没有空闲线程, 则会开启新的线程;  
适用于 耗时短且提交频率高的任务;   
空闲线程最大存活时间60s;
- Executors.newSingleThreadExecutor()
相当于 newFixedThreadPoll(1)  
可以实现多生产者, 单一消费者;  
可以实现任意时刻只有一个线程在执行的任务;
- Executors.ScheduledExectorService()
可以延时执行任务

### 线程的生命周期
线程有六个状态, 是一个state枚举, 包括 NEW, RUNNABLE, WAITING,TIME WAITING, BLOCKED, TERMINATED   
其中RUNNABLE又包含了两个状态, RUNNAING和READY
- 首先NEW一个线程对象, 调用start()之后进入READY, 被调度器选中后进入RUNNING, 调用yield()后又进入READY;
- 从RUNNABLE申请锁会进入BLOCKED, 获得锁后回到RUNNABLE;
- 调用Object.wait()会进入WATTING, Object.notify()回到RUNNABLE
- 调用Thread.join()进入WATTING,等待的线程结束回到RUNNABLE
- 调用sleep(long),或者Object.wait(long)会进入TIME WATTING, 时间结束回到RUNNABLE
- run()方法执行结束, 进入TERMINATED状态

## 2.4 锁机制

### 说说线程安全问题
线程安全问题, 就是要保证我们操作的 原子性 有序性 和可见性, 
- 原子性就是, 一个操作或多个操作, 要么全部被执行, 要么一个也没有执行;
- 有序性就是, 代码执行的顺序是有序的, 因为cpu处理或者代码编译的时候, 会导致重排序;
- 可见性就是, 一个线程修改了变量的值, 其他线程能立马看到这个变动;

### volatile 实现原理
一个共享变量被volatile修饰时, 对于改变量的改动, 能够立即更新到主内存,其他线程读取值得时候, 会去主内存读取该值;

### synchronize 实现原理

### synchronized 与 lock 的区别
synchronized和reentrantLock都是可重入锁, 但是synchronized是隐式锁, lock是显式锁;
- synchronized无法判断是否获取锁的状态，Lock可以判断是否获取到锁；
- synchronized会自动释放锁(a 线程执行完同步代码会释放锁 ；b 线程执行过程中发生异常会释放锁)，Lock需在finally中手工释放锁;
- lock可以使公平锁, synchronized只能是非公平锁

### CAS 乐观锁
- 悲观锁是指 认为其他人都会修改数据 所以每次去拿数据都上锁, 其他想拿数据的只有等拿到锁才行;
- 乐观锁 每次拿数据的时候都认为别人不会修改, 在更新的时候再判断是否有人变动过这个数据;
- CAS是指比较然后交换, 是指在更新前, 先拿到被更新的旧值, 更新时先比较旧值, 如果一致在更新, 利用的是cup的compareAndSet指令

### ABA 问题
- ABA问题是指在CAS时, 旧值被其他线程修改又修改回来了;
- 可以用加版本号的方式, 比较旧值和版本号然后再更新;

### 乐观锁的业务场景及实现方式
java中有原子操作类, 原子操作类都利用了CAS原子锁, 而原子操作类又是concurrent包中很多类的基础;

# 3 核心篇
## 3.1 数据存储
### MySQL 索引使用的注意事项
- is null条件不走索引, 设计表的时候, 列如果确定有索引, 最好设计出not null;
- 在列上进行计算不会走索引;
- not in, <>, != 操作不走索引;
- where 和 join in 中出现的列 要有索引;
- where 条件中使用了函数, 则不会走索引;

### 说说反模式设计
主要是反第三范式
- 第三范式: 一个数据库表中不包含其他表中已经包含的非主键字段;
- 反第三范式: 为了运行效率, 可以存有冗余字段;

### 说说分库与分表设计
- 分库即有读写分离和冷热分;
- 分表有水平分和垂直分, 按照学员id分是水平分, 把各个列拆分成多表, 这是垂直分;

### 分库与分表带来的分布式困境与应对之策
读写分类后, 读库和写库之间的数据同步有延迟, 如果有先写然后立马读的操作, 会出错, 所以应该有缓存, 而且缓存应该写之后立马写缓存, 读的时候应该先读缓存, 缓存没有再去读数据库;

### 说说 SQL 优化之道
- 写完一个较复杂查询, 一定要用explain查看
- 确定没重复的情况下用union all而不是union
- update更新的时候增加limit, 以免sql有误, 更新全表...
- 能一次查完的数据,尽量不要查询多次;
- 如果查出一条数据即可满足, 一定要加limit 1;
- 连表时在join 条件中增加较多的条件, 可以减少连表的数据, 加快速度;

### MySQL 遇到的死锁问题
- mysql的锁有表锁和行锁, innodb支持行锁, 
- 锁又分为两种类型 共享锁和排他锁
- 行锁可能会引起死锁, 为了数据一致性  

会造成死锁的情况:
- 可能会在查询然后更新的操作中,两个session的事务互相持有对方要操作的锁, 造成死锁;
```
sessionA:
select * from xxx where id='1' for update;
select * from xxx where id='2' for update;

sessionB:
select * from xxx where id='2' for update;
select * from xxx where id='1' for update;
```

### 存储引擎的 InnoDB 与 MyISAM
区别:
- InnoDB支持行锁, MyISAM不支持行锁;
- InnoDB支持事务, MyISAM不支持事务;
- MyISAM支持全文搜索, InnoDB不支持全文搜索;
- MyISAM查询速率较高,适合查询较多的场景, InnoDB适合插入更新较多的场景;

### 数据库索引的原理
- 通过索引可以快速的查找到数据,mysql使用B+Tree作为索引结构;
- 对于不同存储引擎, 索引结构不太一样;

### 为什么要用 B-tree
- 数据库的索引是存储在磁盘上的,在利用索引进行查询时, 不可能加载全部索引, 只能逐一加载每个磁盘页, 也就是索引树的节点;
- 索引磁盘IO的次数, 就是存储的树的高度;
- 一个m阶b-tree, 每个节点最多可以拥有m个子树,每个节点可以有m-1个key, 以升序排列
- MyISAM的索引和数据是分离存储的, 索引的叶子节点存储的是数据的地址;
- InnoDB的索引和数据是在一起存储的, 主键索引中的叶子节点包含了完整的数据记录, 这也是为什么InnoDB必须要有主键;
- InnoDB的辅助索引的叶子节点, 存储的是主键的值;


### 聚集索引与非聚集索引的区别
- 聚集索引, 既存储了索引, 也存储了所有数据值, InnoDB的主键索引即为聚集索引;
- 聚集索引是以物理磁盘顺序来存储的;
- 自增主键会把数据自动向后插入,避免了插入过程中索引重排序的问题;

### limit 20000 加载很慢怎么解决

### 选择合适的分布式主键方案
可以使用uuid作为逻辑主键, 数据库自增id作为物理主键;

### 选择合适的数据存储方案
### ObjectId 规则
### 聊聊 MongoDB 使用场景

### 倒排索引
倒排索引即为对文档全文中的单词做索引, 记录每个单词在每个文档中出现的次数;

### 聊聊 ElasticSearch 使用场景
es可以用在 搜索引擎, 全文检索, 数据快速检索, 日志分析等;

- 我们主要用es做数据的快速检索, 我们有学员大表, 可以快速的查询学员的数据;
- 学员的基本数据, 学员的订单信息, 学员的课程信息等;
- 通过job来进行学员大表的更新, 有数据库表记录了上次更新的时间, 然后更新上次记录的时间之后的数据;
- 我们的ES集群有三个节点, 每个索引三个分片, 两个备份;


## 3.2 缓存使用
### Redis 有哪些类型
string, hash, list, set, zset

### Redis 内部结构

### Redis 内存淘汰机制
即redis检查内存, 发现内存使用量大于一个阀值, 则按照一定的策略来清除数据;

### 聊聊 Redis 使用场景
缓存热点数据, 应对高并发, 提升系统性能;
工作中用到redis的地方:
- 缓存试卷以及题目, 试卷形式的题目缓存整套试卷, 题目组合的练习类型缓存每道题目;
- 学员练习排行榜, 缓存了前3000名的排行, 返回前10名及学员自己的排行信息;

### Redis 持久化机制
持久化即为将redis中的数据, 按照指定的策略写入硬盘, 再重启时恢复热点数据;
### Redis 集群方案与实现
### Redis 为什么是单线程的
- redis虽然单线程, 但是可以达到10万qps;
- redis使用了多路I/O复用模型,不阻塞在等待某一个流的过程中;
- CPU不是Redis的瓶颈, 内存或者宽带才是, 所以说不必使用多线程;


### 缓存崩溃
- 缓存崩溃是指原有缓存过期,或者redis服务器出现问题, 导致缓存无法查询,大量请求去查询数据库, 造成数据库慢或者宕机; 
- 缓存穿透是指,用户查询的数据数据库和缓存都没有, 但是用户每次还是需要去数据库查询;

### 缓存降级
目的是保证核心业务可用, 对服务进行降级, 可以拒绝服务或者延迟服务;
- 一个比较常见的做法是, redis出现问题, 不去数据库查, 而是直接返回默认值给用户;

### 使用缓存的合理性问题
- 频繁修改的数据, 看情况再使用缓存;
- 热点数据, 缓存才有价值;
- 缓存会导致一定时间的数据不一致性;

## 3.3 消息队列
### 消息队列的使用场景
消息队列可用于异步处理, 应用解耦, 以及削锋处理;  
工作中用到kafka的地方:
- canal监控数据库表变更, 向业务系统发送消息;
- 外部做题记录同步, 外部系统发kafka消息, 我们消费kafka消息同步做题记录;
- 学员做题之后, 做题记录发kafka消息, 进行学员平均分统计, 做题统计等信息;

### 消息的重发补偿解决思路
### 消息的幂等性解决思路（已解答，待补充）
幂等性: 对一次或者多次请求的结果是一致的;
### 消息的堆积解决思路
### 自己如何实现消息队列
### 如何保证消息的有序性

### kafka的集群
- kafka的消息通过 主体进行分类, 即topic
- 一个独立的kafka服务器成为一个broker
- 一个topic可以有几个分区, 消息在单个分区是有序的, 但是在主体整体不能保证有序;
- 复制系数即每个分区的复制的份数;
- 通常一个topic的分区和复制分区分布在不同的broker
- 消费者消费需要指定group

# 4 框架篇
## 4.1 Spring
### Spring IOC 如何实现
- IOC即控制反转, 将类与类之间解耦, 原理A依赖B时, 需要自己去new 一个B对象, 而有了控制反转之后, A需要B时, 是容器给A一个对象, 不需要A自己去生成;
- 反转即为由主动生成对象, 变成了被动的获取对象;

### BeanFactory 和 ApplicationContext 有什么区别
### Spring Bean 的生命周期
- 由BeanFactory或者ApplicationContext读取Bean的定义文件, 并生成各个实例;
- Setter注入, 执行Bean的属性依赖注入;
- 如果实现了BeanNameAware接口,则执行其setBeanName方法;
- 如果实现了BeanFactoryAware方法, 则执行setBeanFactory()方法(可以用这个方法获取到其他Bean);
- 如果实现了ApplicationContextAware接口, 则会调用setApplicationContext(ApplicationContext)方法,
传入spring上下文;
- 如果Bean关联了BeanPostProcessor接口, 将会调用postProcessBeforeInitialzation方法
- 如果Bean配置了init-method属性, 会调用其初始化方法
- 如果关联了BeanPostProcessor接口,将会调用 postAfterInitialzation方法;

- Bean实现了DisposableBean借口哦, 会调用destory方法
- Bean配置了destory-method属性, 会调用其配置的销毁方法;

### 说说 Spring AOP
AOP即为面向切面编程, 可以在一些方法的执行前后, 插入一些操作, 比如说日志, 事务, 读写分离配置等;

### Spring AOP 实现原理
在注入类的时候, 通过动态代理, 在代理类中invoke方法中, 插入切面的切入的方法逻辑;

### 动态代理（cglib 与 JDK）
- jdk动态代理是利用反射机制生成一个实现代理接口的类,在invoke()方法中插入逻辑;
- cglib动态代理是通过修改class文件的字节码生成子类,覆盖其中的方法来实现的;
- 如果目标对象实现了接口, 则默认情况下会采用jdk的动态代理实现AOP;
- 如果目标对象没有实现接口, 必须使用cglib代理;
- spring会自动在jdk动态代理和cglib直接转换;

### Spring 事务实现方式
- 可以通过xml形式配置实现
- 通过Transactional注解实现

事务的传播行为有:
- PROPAGATION_REQUIRED 支持当前事务, 如果当前没有事务, 就新建一个;
- PROPAGATION_SUPOORTS 支持当前事务, 如果当前没有事务, 就以非事务方式运行;
- PROPAGATION_MANDATORY 支持当前事务, 如果当前没有事务, 就抛异常;
- PROPAGATION_REQUIRES_NEW 新建事务, 如果当前有事务, 则挂起当前事务;
- 等等....

数据库事务的特性有acid, 原子 一致 独立 持久

事务的隔离级别有:
- Read Uncommited 会导致脏读, 事务A会读到事务B未提交的数据;
- Read Commit , 避免脏读, 会导致不可重复读, 事务B修改并提交了数据, 导致事务A两次读到的数据不一致;
- Repeatable Read, 避免不可重复读, 但是会导致幻读, 即事务B虽然不能修改数据, 但是事务B新增或删除了数据, 导致事务A两次读到的数据不一致;
- Serializable 串行操作, 可避免幻读, 但是效率低;
- mysql默认事务级别为 Repeatable Read

### Spring 事务底层原理
通过PlatformTransactionManager来实现, 有commit和rollback方法;
### 如何自定义注解实现功能
- 实现一个注解接口, 类型为@interface;
- 标注作用时间@Retention为RUNTIME
- 标注注解使用的地方@Target, 可以是method, 也可以是参数, 构造
- 定义一个属性值, 并设置默认值;
- 在需要使用注解的地方, 通过反射获取方法的注解, 然后做一些操作;

### Spring MVC 运行流程
- 用户请求到 dispatcherServlet
- dispatcherServlet通过handlerMapping查找处理器, 然后相应给dispatcherServlet;
- DispatcherServlet调用HandlerAdapter去执行处理器, 这个处理器就是我们写的Controller;
- handler处理完, 返回modelAndView给dispatcherServlet;
- dispatcherServlet调用视图解析器ViewResolver进行视图解析, 视图解析器返回view给dispacherServlet
- dispatcherServlet对视图渲染, 并相应用户

### Spring MVC 启动流程
在web.xml中配置dispatcherServlet, dispatcherServlet进行一系列的初始化, 并监听请求;

### Spring 的单例实现原理
### Spring 框架中用到了哪些设计模式
- 代理模式 AOP切面的代理对象
- 单例模式, spring中定义的bean默认为单例模式
- 模板模式 spring data redis 的RedisTemplate是一个操作redis的模板, 代码复用的基本技术
- 工厂模式, BeanFactory来创建对象的实例


### Spring 其他产品（Srping Boot、Spring Cloud、Spring Secuirity、Spring  Data、Spring AMQP 等）
- spring boot 实现了自动配置, 起步依赖;
- springBoot只要配置application.yml以及application.properties文件即可;

## 4.2 Netty
### 为什么选择 Netty
- netty API简单
- 性能高
- 可以切换io以及nio而不用改太多代码

### 说说业务中，Netty 的使用场景
很多rpc框架和消息队列使用了netty

### 原生的 NIO 在 JDK 1.7 版本存在 epoll bug
在linux版本为2.6的情况下, selector.select()方法没有事件时, 应该阻塞,但是由于一些原因, select出的key为0, 导致不断循环导致cpu高;

### 什么是TCP 粘包/拆包
- 客户端发送的两个数据包, 服务端收到可能是一个连续的数据包, 或者有一个数据包被分开了;
- 原因可能是tpc的缓存大小, 数据包的大小加起来小于缓存, 可能导致粘包, 大小大于缓存, 可能导致拆包

### TCP粘包/拆包的解决办法
- 给每个数据包添加首部, 首部记录数据包的长度;
- 每个数据包都等长发送;

### Netty 线程模型
netty的线程模型是一个reactor反应器模型;
- 请求 首先来到 Accept EventLoopGroup, 相当于一个EventLoop的accept pool 线程池,
- EventLoopGroup 为每个新建的channel分配一个EventLoop, 在channel的生命周期内, 所有操作都在相同的线程中执行;
- channel调用各个handler来处理请求;

### 说说 Netty 的零拷贝
netty的零拷贝主要体现在下面几个方面:
- netty接收和发送采用DIRECT BUFFERS, 使用堆外直接内存进行Socket读写;
- netty提供了CompositeByteBuf类, 可以将多个ByteBuffer合并成一个逻辑上的ByteBuf, 避免了内存拷贝合并;

### Netty 内部执行流程
EventLoopGroup -- > EventLoop--> channel --> handler  
- EventLoop用于处理连接生命周期中发生的事件;
- channel 相当于Socket;
- 创建channel, 将channel注册到EventLoop
- channelHandler是处理具体逻辑的地方;

### Netty 重连实现
- 通过发心跳包;
- 客户端断开重连;
# 5 微服务篇
## 5.1 微服务
### 前后端分离是如何做的

### 如何解决跨域
- nginx配置 
- cros

### 微服务哪些框架

### 你怎么理解 RPC 框架
RPC框架其实就是一个远程代理, 让调用远程的服务像调用本地的服务一样方便;

### 说说 RPC 的实现原理
RPC其实就是服务端开启服务,客户端和服务端建立tcp连接,  然后客户端将需要执行的方法和参数传给服务端, 服务端通过动态代理反射获得要执行的方法和参数,执行, 然后将结果返回给客户端;

### 说说 Dubbo 的实现原理

### 你怎么理解 RESTful
- 每个uri代表一种资源


### 说说如何设计一个良好的 API

### 如何理解 RESTful API 的幂等性
### 如何保证接口的幂等性
- 全局唯一id;
- 版本控制;


### 说说 CAP 定理、 BASE 理论
CAP定理指的是 一个分布式系统, 不可能同时满足 一致性 可用性 和 分区容错性, 最多满足其中两项;  
BASE理论指的是, 基本可用, 最终一致性, 核心思想是及时无法做到强一致性, 也可以根据系统自身特点, 使用适当的方式使系统达到最终一致性;

### 怎么考虑数据一致性问题

### 说说最终一致性的实现方案
业务分拆后, 我们通过canal结合mq发通知的形式, 实现数据的一致性;
还有通过消费消息的形式进行数据同步;

### 你怎么看待微服务

### 微服务与 SOA 的区别
### 如何拆分服务
### 微服务如何进行数据库管理
### 如何应对微服务的链式调用异常
### 对于快速追踪与定位问题
### 微服务的安全
## 5.2 分布式
### 谈谈业务中使用分布式的场景

### Session 分布式方案
没有接入单点之前我们是没有做session共享的, 因为是根据nginx配置的是iphash, 后来接入单点系统后, 不做session共享的话, 会有登出问题, 于是用了tomcat-redis-session-manager做了session共享;

### 分布式锁的场景
跨进程或者跨服务器的情境下, java的原生锁无法满足保护共享资源的情况下, 需要分布式锁来保证共享资源的有序使用;

### 分布式锁的实现方案
- 数据库实现
- redis实现 使用SETNX(set if not exist)供能
- zookeeper实现


### 分布式事务
- 可以在A事务的数据库增加消息表,A事务执行成功则生成一条消息, 并向B事务的服务器发送消息, B事务执行成功则返回成功消息给A, A删除消息, 执行失败则重试或回滚;
- 利用rocket mq 来实现分布式事务; mq发送两次消息, 一次发送消息, 一次确认消息, 确认消息失败, 重复发送或回滚;


### 集群与负载均衡的算法与实现
nginx几种负载均衡有:
- 按照时间来均衡; 对所有的机器轮询发送
- 按照weight来均衡服务器; 分析连接数, 将连接分配至负载最轻的, 考虑分配系数;
- iphash均衡; 根据ip计算hash值, 分配到指定服务器;

### 说说分库与分表设计
- 分表可以解决单表数据量过大带来的效率低下; 
- 当数据库的写的能力达到瓶颈时, 增加读库或者分表已经无法解决, 这时候需要分库;
- 分库可以提高数据库并发写的能力;
- 分表可以水平分和垂直分 
- 水平分就是把数据部分在不同的表中, 每条数据依然包含所有字段;
- 垂直分就是按照字段, 将数据的几个字段拆分出来成为一个单表;
- 分库分表都可以按照用户id取模来请求


### 分库与分表带来的分布式困境与应对之策
- 需要统计分库或者分表的数据时, 需要查询多张表; 设计时, 就应该避免这种查询操作;
- 有需要全局唯一id的表, 分表后需要全局唯一id;



## 5.3 安全问题
### 安全要素与 STRIDE 威胁
### 防范常见的 Web 攻击
### 服务端通信安全攻防
### HTTPS 原理剖析
- 服务端和客户端互相发送公钥密钥, 对发送的信息加密;
- 为了避免发送公钥密钥的过程被获取, 通过第三方证书机构的数字证书来交换公钥秘密;

### HTTPS 降级攻击
### 授权与认证
### 基于角色的访问控制
### 基于数据的访问控制

## 5.4 性能优化
### 性能指标有哪些
- GC时间
- 接口的相应时间, sql的响应时间;

### 如何发现性能瓶颈
- gc 通过jstat 查看gc日志;
- 通过sql的explan查看索引命中;

### 性能调优的常见手段
正则表达式不要多次编译;


### 说说你在项目中如何进行性能调优
# 6 工程篇
## 6.1 需求分析
### 你如何对需求原型进行理解和拆分
### 说说你对功能性需求的理解
### 说说你对非功能性需求的理解
### 你针对产品提出哪些交互和改进意见
### 你如何理解用户痛点
## 6.2 设计能力
### 说说你在项目中使用过的 UML 图
### 你如何考虑组件化
### 你如何考虑服务化
### 你如何进行领域建模
### 你如何划分领域边界
### 说说你项目中的领域建模
### 说说概要设计
## 6.3 设计模式
### 你项目中有使用哪些设计模式

### 说说常用开源框架中设计模式使用分析
### 说说你对设计原则的理解
### 23种设计模式的设计理念
### 设计模式之间的异同，例如策略模式与状态模式 的区别
### 设计模式之间的结合，例如策略模式+简单工厂模式的实践
### 设计模式的性能，例如单例模式哪种性能更好。
## 6.4 业务工程
### 你系统中的前后端分离是如何做的
后端提供接口, 前端通过ajax调用, 前后端分开发版, 通过nginx配置到一个域下;

### 说说你的开发流程
- 开需求评审会, 评审需求;
- 然后技术文档编写, 开技术评审;
- 进入开发;

### 你和团队是如何沟通的

### 你如何进行代码评审
### 说说你对技术与业务的理解
### 说说你在项目中经常遇到的 Exception
- invokeTargetException, 反射获取方法执行的时候, exception会被包装成invocationTargetException, 需要调用getTargetException()方法来获取抛出的真正异常;

### 说说你在项目中遇到感觉最难Bug，怎么解决的
- 读写分离及事务问题;
- 验证码识别问题;
- rpc 时常高;


### 说说你在项目中遇到印象最深困难，怎么解决的
### 你觉得你们项目还有哪些不足的地方
代码review做的不够好, 没有引入review的机制;

### 你是否遇到过 CPU 100% ，如何排查与解决
遇到过, rpc项目时常CPU高, 最高达到2000%,... 查看了gc日志, 查看了堆栈信息, 查看了连接数, 没有发现问题, 最后通过更新了 spring data redis 的版本解决, 至今没找到原因;

### 你是否遇到过 内存 OOM ，如何排查与解决
- kafka的send方法中, 每次send都会新建一个producer, 且没有关闭, 每个连接都会占用一个线程,ProducerSendThread, 导致线上有大量的线程, 导致频繁 full gc;
- 解决 将producer的获取改为单例;


### 说说你对敏捷开发的实践
### 说说你对开发运维的实践
### 介绍下工作中的一个对自己最有价值的项目，以 及在这个过程中的角色


## 6.5 软实力
### 说说你的亮点
能专注与任务


### 说说你最近在看什么书
netty in action

### 说说你觉得最有意义的技术书籍
thinking in java, 疯狂的java

### 说说个人发展方向方面的思考
成为技术专家, 能够解决难题;

### 说说你认为的服务端开发工程师应该具备哪些能力
知识面的广度, 还有深入一个面的能力, 专注度, 理解力, 沟通能力;


### 说说你认为的架构师是什么样的，架构师主要做 什么
### 说说你所理解的技术专家
# 7 HR 篇
### 你为什么离开之前的公司
对口产品团队大量离职, 导致接下来的两个月可能都没有需求;

### 你为什么要进我们公司


### 说说职业规划
### 你如何看待加班问题
### 谈一谈你的一次失败经历


### 你觉得你最大的优点是什么
学习能力强, 乐于接受困难, 能够专注于问题的解决

### 你觉得你最大的缺点是什么
正如前面所说, 遇到问题可能会一直深陷进入, 解决不了会有很大的挫败感, 而且可能有事深陷问题, 导致不能站在更高的角度思考;

### 你在工作之余做什么事情
打球, 

### 你为什么认为你适合这个职位
学习能力强, 沟通能力好, 解决问题的能力强;

### 你觉得自己那方面能力最急需提高
接触的业务场景简单, 缺乏在真实环境下的技术思考;

### 你来我们公司最希望得到什么
### 你希望从这份工作中获得什么
### 你对现在应聘的职位有什么了解
### 您还有什么想问的
团队的定位, 有什么发展瓶颈;

### 你怎么看待自己的职涯
### 谈谈你的家庭情况
### 你有什么业余爱好
### 你计划在公司工作多久


# JVM相关问题
### JVM的内存分布
#### 非线程共享区域  
- 程序计数器
记录当前线程所执行的字节码的行号;

- 虚拟机栈
每个方法执行时, 都会创建一个栈帧, 其中包括:  

局部变量表: 保存函数的参数及局部变量;  

操作数栈: 保存计算过程的中间结果;

帧数据区: 包括常量池指针, 以及异常处理表;

- 本地方法栈
为native方法服务

#### 线程共享区域
- 堆
存放对象实例;

- 方法区
包括 类信息  常量 静态变量 运行时常量池 等

- 运行时常量池
包含包装类型, String类型, 等于被引号包括的字符串时, 是在常量池拿对象;

- jdk1.8已经没有方法区, 改为元数据区, 元数据区没有默认大小;


### 如何判断对象已死
- 引用计数法
被引用, 计数+1, 引用失效, 计数-1, 对象相互循环引用问题;

- 可达性分析算法
通过一些根节点, 搜索所走过的路径成为引用链, 一个对象没有任何引用链相连, 则为不可用对象;

- gc roots有哪些
虚拟机栈 以及 本地方法栈中引用的对象; 

方法区中静态属性 常量;

### 有哪些垃圾回收算法
- 标记清除, 效率低, 容易产生内存碎片;
- 复制算法, 可用空间缩小为原有一半;
- 标记整理, 标记出需要回收对象, 所有存活对象向一端移动, 清理掉边界外的对象;
- 分带收集, 分为新生代和老年代, 新生代使用复制算法, 老年代使用标记整理算法

### 有哪些垃圾回收器
Serial , Serial Old, ParNew, Parallel old, Parallel Scavenge, CMS, G1
常用组合有
-  ParNew + CMS + Serial Old
-  Parallel Scanvenge + Parallel Old 吞吐量优先;
-  G1中, 堆被平分为几个区域,每个区域保留了新老代的概念, 新老带


### 为什么需要full GC

### 写一个单例
内部类在第一次使用时才加载, 且只有一个线程可以初始化同一个类;
```
public Class Singleton{
    private Singleton(){}
    
    private static Class SingletonHolder{
        private static Singleton singleton = new Singleton();
    }
    
    public static Singleton getInstance() {
        return SingletonHolder.singleton;
    }
    
    
}

```
