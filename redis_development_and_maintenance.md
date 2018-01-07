《Redis开发与运维》
=======================

## 作者
   磊 / 张益军 
  
## 简介
> 本书全面讲解Redis基本功能及其应用，并结合线上开发与运维监控中的实际使用案例，深入分析并总结了实际开发运维中遇到的“陷阱”，以及背后的原因， 包含大规模集群开发与管理的场景、应用案例与开发技巧，为高效开发运维提供了大量实际经验和建议。本书不要求读者有任何Redis使用经验,对入门与进阶DevOps的开发者提供有价值的帮助。主要内容包括：Redis的安装配置、API、各种高效功能、客户端、持久化、复制、高可用、内存、哨兵、集群、缓存设计等，Redis高可用集群解决方案，Redis设计和使用中的问题，最后提供了一个开源工具：Redis监控运维云平台CacheCloud。

## 作者简介
> 付磊 搜狐视频高级研发工程师，CacheCloud项目联合创始人。拥有多年Redis开发运维经验，为公司多个核心业务提供Redis服务，同时热衷于技术传播和分享，撰写了大量关于Redis开发运维的技术文章。微博号carlosfl，博客地址是http://carlosfu.iteye.com。

> 张益军 搜狐视频资深研发工程师，CacheCloud项目联合创始人，曾就职于美团、阿里巴巴等公司。搜狐视频投放组负责人，目前从事投放平台、反作弊等系统的架构设计和优化工作。研究兴趣包括海量峰值访问、分布式存储等。微博号益军YJ, 博客地址是http://hot66hot.iteye.com。

## 豆瓣链接
[豆瓣链接](https://book.douban.com/subject/26971561/)

## 目录

## 内容

#### 第1章 初识Redis
* 特性：速度快、基于键值对的数据结构服务器、丰富的功能、简单稳定、客户端语言多、持久化、主从复制、高可用和分布式
* 使用场景：缓存、排行榜、计数器、社交网络、消息队列
* 不适用场景：超过内存的大数据量、冷数据 

#### 第2章 API的理解和使用
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

#### 第3章 小功能大用处
* 附加功能：慢查询分析、Redis Shell、Pipline、事务与Lua脚本、Bitmaps、HyperLogLog、发布订阅、GEO
* 慢查询分析：
  - config set sloglog-log-slower-than 20000
  - config set slowlog-max-len 1000
  - config rewrite // 将配置持久化到本地配置文件
  - slowlog get [n] // 查持慢查询日志
  - slowlog len
* Redis Shell
  - redis-cli -r 100 -i 1 info | grep used_memory_human
  - redis-cli --slave // 模拟从节点获取Redis更新操作
  - redis-server --test-memory 1024  // 检测系统能否提供1G内存
  - redis-benchmark -c 10 -n 200 -r 1000
* 事务与Lua
  - multi, [order...], exec // 事务开始与结束
  - Redis不支持回滚功能
* 发布订阅
  - publish channel message
  - subscribe channel 
  - 简单，相比专业的消息队列系统，无法实现消息堆积和回溯
  

#### 第5章 持久化
* RDB和AOF两种持久化机制
  - RDB数据快照，有手动和自动触发两种
    - save阻塞，不建议用于线上；bgsave fork子进程
    - 特点：适用于备份、加载快于AOF，成本高，二进制格式保存，不适合实时持久化
  - AOF（append only file）独立日志
    - 采用文本格式：兼容性好、可读可处理
    - 命令追加到aof_buf中
    - 重写机制：把Redis进程内的数据转化为写命令同步 到新AOF文件的过程
      - 压缩文件体积（除去了超时数据、无效命令、多条写命令合并）
      - 手动、自动触发 
* 启动时优先加载AOF
* 影响性能的高发地
  - fork操作：每GB消耗20毫秒
  - 子进程开销：CPU、内存、硬盘
  - AOF追加阻塞：
  - 多实例部署：

#### 第6章 复制
*  

#### 第7章 Redis的噩梦：阻塞
* 阻塞原因及解决
  - 内因：
    - 不合理使用API或数据结构
      - 查看慢查询日志
      - 修改算法复杂度
      - 拆分大对象（redis-cli --bigkeys）
    - CPU饱和
      - 使用top命令查看，统计命令redis-cli --stat
    - 持久化阻塞
      - 引起阻塞操作：fork阻塞、AOF阻塞、HugePage写操作阻塞
  - 外因：
    - CPU竞争：不和其它CPU密集型服务部署在一起
    - 内存交换
      - redic-cli info server | grep process_id
      - cat/proc/{process_id}/smaps | grep Swap
      - 保证内存并降低系统使用swap优先级
    - 网络问题
      - 连接拒绝：闪断？拒绝？溢出？
      - 连接数：redis-cli info Stats | grep rejected_connections
      - ulimit -n 进程使用资源限制
      - backlog队列溢出： netstat -s | grep overflowed
      - 网络延迟： redis-clic --latency， --latency-dist

      
* 发现阻塞
  - 借助日志及报警 
  - 监控指标：命令耗时、慢查询、持久化阻塞、连接拒绝、CPU/内存/网络/磁盘过载


#### 第8章 理解内存
* 内容：内存消耗分析、管理内存的原理与方法、内存优化技巧
* 内存消耗
  - info memory
    - 关注used_memory_rss
    - used_memory 自身内存、对象内存、缓冲内存
    - mem_fragmention_ratio（正常1.03左右。<1，交换到硬盘变慢，重点关注）
      - 高碎片场景：频繁更新、大量过期键删除
      - 高碎片解决方式：数据对齐、安全重启
    - 子进程消耗
      - copy-on-write，高并发场景开启THP时，子进程消耗甚巨
* 内存管理
  - 控制内存上限
    - config set maxmemory 4GB 实际使用内存，未计算碎片内存
  - 回收策略
    - 删除过期键对象
      - 惰性删除：客户端读取时检查并删除
      - 定时任务删除：默认每秒10次
    - 达到maxmemory上限时溢出控制策略
* 内存优化
  - 封闭为redisObject对象
    - type, encoding, lru, refcount（引用计数）,ptr， 
  - 缩减键值长度：高压缩比序列化工具
  - 共享对象池:复用内部维护的[0-9999]整数对象池（object refcount [key]）


#### 第11章 缓存设计
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