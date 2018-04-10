[TOC]

[整理](https://blog.csdn.net/a895458278/article/details/78655461)

*斜体为目前没掌握的*

## 基础
* 数组，链表，栈和队列
* 树，二叉树，平衡二叉树，二叉搜索树
* 二叉树的前序，中序，后序遍历
* 排序算法（冒泡，快排，归并，堆等）。描述区别及手写

## 算法
* 回溯，贪心，动态规划等
* 最长公共子序列

## 数据库
* innodb 和 myisam的区别
* innodb记录及索引组织方式
* B树和B+树
* B+树的增和删算法
* mysql索引
* mysql优化 - explain
* *非关系型数据库：hbase, monogodb等* 数据库隔离级别， ACID
* 悲观锁，乐观锁 及业务场景
* 分库分表（水平拆分、垂直拆分）
  *分库分表带来的分布式困境和应对之策*
* 聚集索引和非聚集索引（聚集索引：一级索引；非聚集索引：二级索引）
* limit 100000问题的解决方式
* 分布式主键的选择方案
* *elasticSearch使用场景*
* *非关系型数据库如mongoDB*

## 网络
* 网络分层：4层和7层
* TCP：三次握手，四次挥手 - 为什么及讲清楚
* UDP和TCP区别
* *TCP／IP 拥塞控制，滑动窗口，如何实现可靠*
* http协议（无状态，请求头和请求体，响应头和响应体）

## Java基础
* 重载／重写，接口／抽象类
* IO、NIO
* 多线程
* 反射
* 集合框架
* Object类的9个方法
* wait和sleep的区别
* 自定义注解（target - METHOD,TYPE,FIELD等,retention - SOURCE,CLASS,RUNTIME）场景和实现
* equals和hashcode
* session和cookie
* *JDBC流程*

## 源码
* hashMap, hashTable, concurrentHashMap
	- hashmap多线程下的死循环问题
	- concurrentHashMap 1.7（segment分段锁，锁粒度减小）和1.8(node+链表+红黑树)实现方式的区别
* *LinkedHashMap, LinkedHashSet, 
  CopyOnWrite*
* *ReentrantLock, CountdownLatch, CyclicBarrier,Condition, AQS*

## 并发
* synchronize 和 lock
* wait /notify, await/signal
* yield, join
* volatile深入理解，CAS理解(ABA问题)
* 锁：自旋锁，偏向锁，可重入锁
* threadPool 和 blockingQueue
* ThreadLocal使用和内存泄漏问题（weakReference）
* *Fork/Join框架的理解*

**书籍：Java编程思想**

## JVM
* 类加载机制及其破坏
* 强／软／弱／虚引用
* Java内存模型
* GC 及 GC算法
* OOM错误，statckoverflow错误，permgen space错误
* JVM调优参数
* *常用垃圾收集器：G1, CMS等*
* 性能监控：jmap, visualVM

## spring
* beanFactory和applicationContext
* spring bean循环注入问题
* IOC原理
* AOP原理 （动态代理 jdkdynamic,cglib）
* 事务原理, 事务管理机制（required和requires_new区别等）
* spring mvc(model, view, controller)，mvc设计思想, 启动／运行流程
* *spring boot, cloud*

## 设计模式
*23种 - todo*

* 重点
 	- 单例模式
 	- 代理模式
 	- 装饰器模式（IO）
 	- 适配器模式

## 安全& 性能
### 安全
 * https原理
 * 常见的web攻击及防范措施
 * 授权与认证

### 性能
* 性能指标
* 性能瓶颈的发现
* 调优常见手段
* 如何进行调优

## 架构学习
### 分布式
* CAP和BASE理论
* 分布式如何保证一致性
* 负载均衡
* 接口幂等性的实现方式
* session分布式
* 分布式锁的实现方式

###微服务
* SOA和微服务
* 谈谈restful风格，restfulAPI的幂等性（post非幂等，put幂等）
* 如何拆分服务


### 框架
* redis
	- redis的5种数据结构（底层及使用场景）：
		- string
		- hashMap
		- list
		- set
		- zset
	- redis实现分布式锁 setnx
	- 缓存实效策略
	- 数据淘汰策略
	- 集群部署（宕机如何处理）
* zookeeper
	- 原理，适用场景
	- 选举策略
	- watch机制
* rpc框架: thrift
* 消息框架：rocketmq
* netty框架
	- 使用场景
	- TCP粘包／拆包 及 解决方法
	-  *线程模型*
	-  *零拷贝*
	-  *重连实现*


## Books
* 《高性能mysql》- doing
* 《HTTP权威指南》
* 《how tomcat works》
* 《JAVA并发编程实战》
* 《深入理解JVM虚拟机》
* 《编程之美》
* 《剑指offer》
* 《TCP／IP详解，卷一：协议》
* 《白帽子讲Web安全》
* 《java编程思想》 - done

## 算法
* 推荐算法 - P0拾起来
* 实践