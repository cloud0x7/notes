《Redis开发与运维》
=======================

### 作者
   磊 / 张益军 
  
### 简介
> 本书全面讲解Redis基本功能及其应用，并结合线上开发与运维监控中的实际使用案例，深入分析并总结了实际开发运维中遇到的“陷阱”，以及背后的原因， 包含大规模集群开发与管理的场景、应用案例与开发技巧，为高效开发运维提供了大量实际经验和建议。本书不要求读者有任何Redis使用经验,对入门与进阶DevOps的开发者提供有价值的帮助。主要内容包括：Redis的安装配置、API、各种高效功能、客户端、持久化、复制、高可用、内存、哨兵、集群、缓存设计等，Redis高可用集群解决方案，Redis设计和使用中的问题，最后提供了一个开源工具：Redis监控运维云平台CacheCloud。

### 作者简介
> 付磊 搜狐视频高级研发工程师，CacheCloud项目联合创始人。拥有多年Redis开发运维经验，为公司多个核心业务提供Redis服务，同时热衷于技术传播和分享，撰写了大量关于Redis开发运维的技术文章。微博号carlosfl，博客地址是http://carlosfu.iteye.com。

> 张益军 搜狐视频资深研发工程师，CacheCloud项目联合创始人，曾就职于美团、阿里巴巴等公司。搜狐视频投放组负责人，目前从事投放平台、反作弊等系统的架构设计和优化工作。研究兴趣包括海量峰值访问、分布式存储等。微博号益军YJ, 博客地址是http://hot66hot.iteye.com。

### 豆瓣链接
[豆瓣链接](https://book.douban.com/subject/26971561/)

### 目录

### 摘要 

### 内容


##### 第2章 API的理解和使用
* Redis数据结构：string, hash, list, set, zset
* 每种数据结构都有底层的内部编码实现，而且是多种实现
  - 通过object encoding [key]查看
  - list就有linkedlist和ziplist两种 
  - 好处：改进内部编码，不同场景下发挥最大优势
* 单线程架构
  - 所有命令进行执行队列
  - 为什么这么快？纯内存访问、非阻塞I/O、单线程避免切换和竞态消耗
* 数据结构
  - string
    - 命令：set, get, del, setex, setnx, exists, mset, mget, incr, append, strlen, getset, 
    - 内部编码： int, embstr, raw
    - 使用场景：缓存、计数、共享session、限速
  - hash
    - 命令：hset, hget, hdel, hmset, hmget, hexists, hkeys, hvales, hgetall, hincrby, hstrlen, 
    - 内部编码：ziplist 少, hashtable 多
    - 使用场景：模拟关系型数据库
  - list
    - 命令：rpush, linsert, lrange, lindex, llen, lpop, ltrim, lrem , lset, blpop
    - 内部编码：ziplist, linkedlist
    - 使用场景：消息队列、文章列表、模拟栈和队列
  - set
    - 命令：sadd, srem, scard, sismember, spop, smembers, sinter, sunion, sdiff, 
    - 内部编码：intset, hashtable
    - 使用场景：标签、随机数抽奖、社交互动
  - zset
    - 命令：zadd, zcard, zscore, zrank, zrevrank, zrem, zincrby, zcount
    - 内部编码： ziplist, skiplist
    - 使用场景：排行榜
* 键管理
  - 命令：type, del, object, exists, expire, rename, renamenx, ttl
  - Redis不支持二级数据结构过期
  - 遍历键：keys, dbsize, scan（解决阻塞问题）, hscan, sscan, zscan, 
* 数据库管理
  - select dbIndex
  - 默认配置中有16个数据库


##### 第11章 缓存设计
* 缓存成本
  - 数据不一致性
  - 代码维护成本、运维成本
* 更新策略
  - 策略
    - LRU/LFU/FIFO算法剔除：一致性差，维护成本低
    - 超时删除：设置过期时间
    - 主动更新：一致性强，维护成本高
  - 实践建议
    - 低一致性业务配置最大内存和算法剔除
    - 高一致性业务使用超时和主动更新
* 粒度控制方法
  - 根据数据通用性、空间占用比、代码维护性三点考虑
* 穿透问题：查询一个不存在的数据，缓存层和存储层都不会命中
  - 原因：代码错误 或 恶意攻击
  - 解决：
    - 缓存空对象，但设置短过期
    - [布隆过滤器](https://en.wikipedia.org/wiki/Bloom_filter)
* 无底洞问题
  - 更多的节点不代表更高的性能（网络时间）
* 雪崩问题
  - 保证缓存层高可用
  - 降级
* 热点key重建：缓存失效后大量请求重建缓存
  - 解决
    - 互斥锁
    - 永不过期（逻辑控制更新）