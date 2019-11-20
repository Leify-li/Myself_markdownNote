## MySQL储存原理

 **描述**

> 　　存储过程是一组可以完成特定功能的SQL语句集，经编译后存储在数据库中statement语句（DDL、DML、导出及管理语句等）、异常处理、流程控制 

 **创建存储过程** 

> 　　系统做语句分析，如果没有出现词、语法等的错误则存入系统表mysql.proc中 

**调用存储过程**

1. 编译

   > 　　存储过程体中的语句集会被解析为一组指令集
   > 　　存储过程的执行就是从该组指令集的第一个指令开始顺序执行
   > 　　系统对存储过程的解析是以语句或结构为单位的，该部分处理在YACC中完成
   > 　　注意：语句和指令之间并非一对一的对应关系 



![img](https://images2015.cnblogs.com/blog/538517/201606/538517-20160608213619105-1778875292.png)

2.  优化 
3.  执行 
4.  基本流程示意 

![img](https://images2015.cnblogs.com/blog/538517/201606/538517-20160608214251996-301940000.png)

## MySQL事务

#### **事务具有ACID的特性：**

1. **原子性(atomicity)**:事务中的所有操作是不可分割的，要么全部提交成功，要么全部失败回滚。

2. **一致性(consistency)**:数据库总是从一个一致性状态转换到另一个一致性状态。

3. **隔离性(isolation)** :一个事务所做的修改在提交之前对其它事务是不可见的。

4. **持久性(durability)**:一旦事务提交，其所做的修改便会永久保存在数据库中。

## Redis与MySQL的区别

```python
1.'redis'是内存数据库，数据保存在内存中，速度快。
2.'mysql' 是关系型数据库，持久化储存，存放在磁盘中。检索的话，涉及到一定的IO，数据访问也就慢
```

##### mysql 和redis的高可用性

**mysql**

- **MySQL Replication**

```python
1.MySQL Replication 是 MySQL 官方提供的主从同步方案，
用于将一个 MySQL 实例的数据，同步到另一个实例中。
2.Replication 为保证数据安全做了重要的保证，也是现在运用 最广的 'MySQL 容灾方案'。
3. Replication 用两个或以上的实例搭建了'MySQL 主从复制集群'
```

##### Redis

- **Sentinel** (哨兵)

```python
#Redis官方为集群提供的高可用解决方案
1. 在 实际项目中可以使sentinel 去做 redis 自动故障转移，减 少人工介入的工作量。
2. 另外 sentinel 也给客户端提供了监控消息的通知，这样客户端就可根据消息类型去判断服务器的状态， 去做对应的适配操作。
```

**Sentinel主要功能列表**：

- **Monitoring：** (监察)

```python
Sentinel 持续检查集群中的master，slave状态，判断是否存活
```

- **Notification** 

```python 
在发现某个redis实例挂掉的情况下，Sentinel能通过API'通知'系统管理员或其他程序脚本
```

- **Automatic failover** (自动故障转移)

```python
如果一个master挂掉后，sentinel立马启动故障转移，把某个slave提升为master。其他slave立马重新配置指向新的master
```

- **Configuration provider** (提供设置)

```python
对于客户端来说，sentinel通知是信赖的。
客户端会连接sentinel去请求当前master的地址，一旦发生故障sentinel会'提供'新地址给客户端
```

## Redis/mongodb 优缺点

```python
1. MongoDB 和 Redis 都是 NoSQL，采用结构型数据存储。
2. 二者在使用场景中，存在一定的区别，这也主要由于二者在内存 映射的处理过程，持久化的处理方法不同。
3. MongoDB 建议'集群 '部署，更多的考虑到集群方案，
4. Redis 更偏重于进程顺序写入， 虽然支持集群，也'仅限'于'主-从模式'
```

##### **Redis**

**优点**

```python
1 读写性能优异 
2 支持数据持久化，支持 'AOF' 和 'RDB' 两种持久化方式 
3 支持主从复制，主机会自动将数据同步到从机，可以进行读写分离。
4 数据结构丰富：除了支持 string 类型的 value 外还支持 string、hash、set、sortedset、list 等数据结构。
```

**缺点**

```python
1 Redis' 不具备自动容错和恢复功能'，主机从机的宕机都会导致前端部分读写请求失败，需要等待机器重启或者手动切换前端的IP才能恢复。 
2 '主机宕机'，宕机前有部分数据未能及时同步到从机，切换 IP 后还会引入数据不一致的问题，'降低了系统的可用性'。 
3 Redis的'主从复制采用全量复制'，复制过程中主机会'fork '出一个子进程对内存做一份快照，并将子进程的内存快照保存 为文件发送给从机，这一过程需要确保主机有足够多的空余内 存。若快照文件较大，对集群的服务能力会产生较大的影响， 而且复制过程是在从机新加入集群或者从机和主机网络断开重 连时都会进行，也就是网络波动都会造成主机和从机间的一次 全量的数据复制，这对实际的系统运营造成了不小的'麻烦'。 
4 'Redis 较难支持在线扩容'，在集群容量达到上限时在线 扩容会变得很复杂。为避免这一问题，运维人员在系统上线时 必须确保有足够的空间，这对资源造成了很大的浪费。
```





## 数据库怎么优化查询效率

```python
1.'储存引擎选择'：如果数据表需要事务处理，应该考虑使
用 InnoDB，因为它完全符合 ACID 特性。如果不需要事务处理， 使用默认存储引擎 MyISAM 是比较明智的 2.'分表分库，主从'， 
3.'对查询进行优化'，要尽量避免全表扫描，首先应考虑在where 及 order by 涉及的列上建立索引 
4.应尽量避免在 'where 子句中对字段进行 null 值判断'， 否则将导致引擎放弃使用索引而进行全表扫描
5.应尽量避免在 where 子句中使用' != 或 <> 操作符'， 否则将引擎放弃使用索引而进行全表扫描
6.应尽量避免在 where 子句中使用 or 来连接条件，如果 一个字段有索引，一个字段没有索引，将导致引擎放弃使用索 引而进行全表扫描 
7.Update 语句，如果只更改 1、2 个字段，不要 Update 全部字段，否则频繁调用会引起明显的性能消耗，同时带来大 量日志 
8.对于多张大数据量（这里几百条就算大了）的表 JOIN，要先分页再 JOIN，否则逻辑读会很高，性能很差
```



## 数据库优化方案

```python
1. 优化'索引'、SQL 语句、分析慢查询； 
2. 设计表的时候严格根据数据库的设计'范式'来设计数据库；(三范式)
3. '使用缓存'，把经常访问到的数据而且不需要经常变化的 数据放在缓存中，能节约磁盘 IO； 
4. '优化硬件'；采用 SSD，使用磁盘队列技术 (RAID0,RAID1,RDID5)等； 
5. 采用 MySQL 内部'自带的表分区'技术，把数据分层不同 的文件，能够提高磁 盘的读取效率； 
6. '垂直分表'；把一些不经常读的数据放在一张表里，节约磁盘 I/O；
7. '主从'分离读写；采用主从复制把数据库的读操作和写入 操作分离开来；
8. '分库分表分机器'（数据量特别大），主要的的原理就是数据路由；
9. 选择合适的表'引擎'，参数上的优化； 
10. 进行架构级别的缓存，'静态化和分布式'；
11. '不'采用全文索引； 
12. 采用更快的存储方式，例如 NoSQL 存储经常访问的数
```

## 数据库负载均衡

` 负载均衡集群是由一组相互独立的计算机系统构成，通过 常规网络或专用网络进行连接，由路由器衔接在一起，各节点 相互协作、共同负载、均衡压力，对客户端来说，整个群集可 以视为一台具有超高性能的独立服务器 ` 

##### 1、实现原理

> 实现数据库的负载均衡技术，首先要有一个可以控制连接==数据库的控制端== 。在这里，它截断了数据库和程序的直接连接， 由所有的程序来访问这个**中间层**，然后再由中间层来访问数据 库。这样，我们就可以具体控制访问某个数据库了，然后还可 以根据数据库的当前负载采取有效的均衡策略，来**调整每次连接**到哪个数据库。

##### 2、实现多据库数据同步

> 对于负载均衡，最重要的就是所有服务器的**数据**都是==实时 同步==的。

> > 这是一个集群所必需的，因为，如果数不据实时、不 同步，那么用户从一台服务器读出的数据，就有别于从另一台 服务器读出的数据，这是不能允许的。所以必须实现数据库的 数据同步。

> 这样，在查询的时候就可以有多个资源，==实现均衡==。
>
> > 比较常用的方法是 Moebius for SQL Server 集群，Moebius for SQL Server 集群采用将核心程序驻留在每个机器的数据库中的 办法，这个核心程序称为 Moebius for SQL Server 中间件， 
>
> **主要作用**是监测数据库内数据的变化并将变化的数据**同步**到**其 他数据库中**。数据同步完成后客户端才会得到响应，同步过程 是**并发**完成的，所以同步到多个数据库和同步到一个数据库的 时间基本相等；
>
> 另外同步的过程是在==事务==的环境下完成的，保证了多份数据在任何时刻数据的**一致性**。
>
> 正因为 Moebius 中间 件宿主在数据库中的创新，让中间件不但能知道数据的变化， 而且知道引起数据变化的 SQL 语句，==根据 SQL 语句的类型智能== 的采取不同的数据同步的策略以保证数据同步成本的最小化。
>
> > 数据条数很**少**，数据内容也不大，则**直接同步**数据 数据条数很少，
> >
> > 但是里面包含**大**数据类型，比如文本，二 进制数据等，则先对数据进行**压缩然后再同步**，从而减少网络 带宽的占用和传输所用的时间。
> >
> > 数据条数很**多**，此时中间件会拿到造成数据变化的 **SQL 语句**， 然后对 SQL 语句进行解析，分析其执行计划和执行成本， 并选择是同步数据还是同步 SQL 语句到其他的数据库中。
>
> >  此种 情况应用在对表结构进行调整或者批量更改数据的时候非常有 用。

##### 3、优缺点

**优点**：

```python
(1) 扩展性强：当系统要更高数据库处理速度时，只要简 单地增加数据库服务器就 可以得到扩展。
(2) 可维护性：当某节点发生故障时，系统会自动检测故 障并转移故障节点的应用，保证数据库的持续工作。
(3) 安全性：因为数据会同步的多台服务器上，可以实现 数据集的冗余，通过多份数据来保证安全性。另外它成功地将 数据库放到了内网之中，更好地保护了数据库的安全性。
(4) 易用性：对应用来说完全透明，集群暴露出来的就是
一个 IP
```

**缺点**：

```python
(1) 不能够按照 Web 服务器的处理能力分配负载。 
(2) 负载均衡器(控制端)故障，会导致整个数据库系统瘫痪。
```

## 怎样解决海量数据的存储和访问造成系统设计瓶颈的问题？

```python
1. 水平切分数据库：
	可以降低单台机器的负载，同时最大限度的降 低了宕机造成的损失；分库降低了单点机器的负载；分表，提高了数 据操作的效率， 
2. 负载均衡策略：
	可以降低单台机器的访问负载，降低宕机的可能性；
3. 集群方案：
	解决了数据库宕机带来的单点数据库不能访问的问题； 
4. 读写分离策略：
	最大限度了提高了应用中读取数据的速度和并发
量；
```

### MySQL 集群的优缺点

##### 优点：

 ==a) 99.999%的高可用性== 

b)快速的自动失效切换

 c)灵活的分布式体系结构，没有单点故障

 d)**高吞吐量和低延迟** 

e)可扩展性强，支持在线扩容

#####  缺点： 

a)存在很多限制，比如：**不支持外键** 

b)部署、管理、配置很**复杂** 

c)**占用磁盘空间大，内存大** 

d)备份和恢复不方便

e)重启的时候，数据节点将数据 load 到内存需要很**长时间**

### redis 的使用场景有哪些？

```python
1.取最新 N 个数据的操作
2.排行榜应用,取 TOP N 操作 3.需要精准设定过期时间的应用 4.计数器应用 5.uniq 操作,获取某段时间所有数据排重值 6.Pub/Sub 构建实时消息系统 7.构建队列系统 8.缓存
```

### 怎样解决数据库高并发的问题？(简)

```python
1. 分表分库 
2. 数据库索引 
3. redis 缓存数据库 
4. 读写分离 
5. 负载均衡集群：将大量的并发请求分担到多个处理节点。由于单个处理节点的故 障不影响整个服务，负载均衡集群同时也实现了高可用性
```

### redis 默认端口，默认过期时间，Value 最多可以容纳的 数据长度？ 

##### 默认端口：6379

#####  默认过期时间：

可以说==永不过期==，一般情况下，当配置中开启了超出最大内存限制就写磁盘的话，那么没有设置过期时间的 key 可能会被写到磁盘上。假如没设置，那么 REDIS 将使用 LRU 机制，将内存中的老数据删除，并写入新数据。

#####  Value 

最多可以容纳的数据长度是：**512M**。

## sqlserver，MySQL ，Oracle ，http，redis，https默认端口号？

```livescript
\sqlserver：1433 
\MySQL：3306
\Oracle ：1521 
\http：80
\https：443 
\redis：6379
```

## redis 缓存命中率计算？

Redis 提供了 INFO这个命令，能够随时监控服务器的状态，只用 telnet 到对应服务器的 端口，执行命令即可：

 `telnet localhost 6379`

 `info`
在输出的信息里面有这几项和缓存的状态比较有关系： keyspace_hits:14414110

> 1.keyspace_misses:3228654 
>
> 2. used_memory:433264648 
> 3. expired_keys:1333536 
> 4. evicted_keys:1547380

通过计算 hits 和 miss，我们可以得到缓存的命中率：14414110 / (14414110 + 3228654) = 81% ，

一个缓存失效机制，和过期时间设计良好的系统，命中率可以做到 95%以上