---
title: Redis之命令篇
date: 2019-11-26 21:50:45
tags: [redis, php]
categories: [redis]
keywords: redis命令
toc: true
description: "&emsp;&emsp;本篇主要是梳理redis的常用的API、使用场景、优缺点、时间复杂度等。"
---
## redis常用数据结构
* string 字符串
* hash 哈希
* list 列表
* set 集合
* zset 有序集合
## 通用API
   * 失效时间相关设置 key 的过期时间， 成功返回 1，key 不存在或设置失败，返回 0
    - EXPIRE key seconds 以秒计
    - EXPIREAT key timestamp 参数是 UNIX 时间戳(unixtimestamp)
    - PEXPIRE key milliseconds  以毫秒计
    - PEXPIREAT key milliseconds-timestamp 过期时间的时间戳(unix timestamp) 以毫秒计
   * MOVE key db 
   >将当前数据库的 key 移动到给定的数据库 db 当中
   * PERSIST key 
   >移除 key 的过期时间，key 将持久保持
   * PTTL key 
   >以毫秒为单位返回 key 的剩余的过期时间
   * RANDOMKEY 
   >从当前数据库中随机返回一个 key    
   * RENAME key newkey 
   >修改 key 的名称
   * RENAMENX key newkey 
   >仅当 newkey 不存在时，将 key 改名为 newkey 
   * TYPE key 
   >返回 key 所储存的值的类型
   **以下时间复杂度 O(N)**
   * keys 
   >遍历所有 key (<font color=#dd0000>线上不用</font>)
   * KEYS pattern 
   >查找所有符合给定模式( pattern)的 key (<font color=#dd0000>线上不用</font>)
   * dbsize 
   >计算 key 的总数(<font color=#dd0000>线上不用</font>)  
## String类型API
**以下时间复杂度 O(1)**
- get、set、del 
  >获取、设置、删除
- EXISTS key 
  >检查给定 key 是否存在，若 key 存在返回 1 ，否则返回 0 
- incr key
  > key 自增 1，如果 key 不存在，自增后 get(key)=1
- decr key
- incrby key k
- decrby key k
- set key value
  > 不管 key 是否存在，都设置
- setnx key value
  > key 不存在，才设置
- set key value xx
  > key 存在，才设置
- getset key newvalue 
  > set key newvalue 并返回旧的 value
- append key value 
  > 将 value 追加到旧的 value
- strlen key 
  > 返回字符串的长度（注意中文）
- incrbyfloat key 3.5 
  > 增加 key 对应的值 3.5（没有自减命令，可用 - 号）
- getrange key start end 
  > 获取字符串指定下标所有的值
- setrange key index value 
  > 设置指定下标所有对应的值
**以下 API 时间复杂度 O(N)**
- mget key1 key2 key3... 
  > 批量获取 key，原子操作
- mset key1 value1 key2 value2 key3 value3 
  > 批量设置
  > n 次 get = n 次网络时间 + n 次命令时间
  > 1 次 mget = 1 次网络时间 + n 次命令时间
## hash哈希
   **特点：Mapmap、Small redis、field 不能相同，value 可用相同**
   * hget key field 
   >获取 hash key 对应的 field 的 value
   * hset key field value 
   >设置 对应的 field 的 value
   * hexists key field 
   >判断 hash key 是否有 field
   * hlen key 
   >获取 hash key field 的数量
   * hmget key field1 field2 ... fieldN 
   >批量获取 hash key 的一批 field 对应的值 O(n)
   * hmset key field1 value1 field2 
   >批量设置 hash key 的一批 field value valueN O(n)
   * hgetall key 
   >返回 hash key 对应所有的 field 和 value O(n) <font color=#dd0000>（谨慎使用）</font>
   * havals key 
   >返回 hash key 对应所有的 value O(n)
   * hkeys key 
   >返回 hash key 对应所有的 field O(n)
   * hsetnx key field value 
   >设置 hash key 对应 field 的 value（如 field 已经存在，则失败）
   * hincrby key field 
   >intCounter hash key 对应的 field 的 value 自增 intCounter
   * hincrbyfloat key field 
   >floatCounter hincrby 浮点数版
## list队列
   **特点：有序、重复、左右两边插入弹出(一个列表最多存储 2 ^ 32 -1 个元素)**
   * push key value1 value2 ... valueN  
   >从列表右端插入值（1-N 个） O(1~n)
   * lpush key value1 value2 ... valueN 
   >从列表左端插入值（1-N 个）O(n)
   * linsert key before|after value newValue 
   >在 list 指定的值前|后插入 newValue
   * lpop key 
   >从列表左侧弹出一个 item O(1)
   * rpop key 
   >从列表右侧弹出一个 item    
   * lrem key count value O(n)
     - 根据 count 值，从列表中删除所有 value 相等的项 
     - count > 0，从左到右，删除最多 count 个 value 相等的项
     - count < 0，从右到左，删除最多 Math.abs(count) 个 value 相等的项
     - count = 0，删除所有 value 相等的项
   * ltrim key start end  
   >按照索引范围修剪列表 O(n)
   * lrange key start end（包含 end）
    >获取列表指定索引范围所有 item （获取全部（0 -1））O(n)
   * lindex key index  
   >获取列表指定索引的 item O(n)
   * blpop key timeout 
   >O(n) lpop 阻塞版本，timeout 是阻塞超时时间，timeout = 0 为永远不阻塞（一直等）
   * brpop key timeout 
   >rpop 阻塞版本，timeout 是阻塞超时时间，timeout = 0 为永远不阻塞
## set集合API
   **特点：无序、无重复、集合间操作**
   - Set 是 String 类型的无序集合。集合成员是唯一的，这就意味着集合中不能出现重复的数据。
   - Redis 中集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是 O(1)。
   - 集合中最大的成员数为 232 - 1 (4294967295, 每个集合可存储40多亿个成员)。
   - 假如集合 key 不存在，则创建一个只包含添加的元素作成员的集合
   **常用API**
   - sadd key element 
   >向集合添加一个(O(1) )或多个成员(O(N) )
   - srem key element 
   >移除集合中一个成员 O(1) 
   - SCARD key  
   >获取集合的成员数
   - sismember 
   >集合中是否存在某个元素
   - spop 
   >从集合中弹出一个
   - sdiff 
   >差集
   - sinter 
   >交集
   - sunion  
   >并集
   **<font color=#dd0000>谨慎使用<font>**
   - srandmember 
   > 返回多个元素(谨慎使用)
   - SMEMBERS key 
   > <font color=#dd0000>小心使用,数量太多不行<font>，可scan迭代
   - SSCAN key cursor 
   > 迭代集合中的元素
## zset集合(有序)API
   **特定：无重复元素、有序、element + score**
   * zadd key score element 
   >添加 score(可以重复) 和 element（不可以重复） O(logN)
   * zrem key element 
   >删除元素 O(1)
   * zscore key element 
   >返回元素的分数 O(1)
   * zrevrange key start stop
   >返回有序集中指定区间内的成员，通过索引，分数从高到低
   * zrevrank key member 
   >返回有序集中指定区间内的成员，通过索引，分数从高到低
   * zincrby key score 
   >增加或减少元素的分数 O(1)
   * zcard key 
   >返回元素的总个数 O(1)
   * zrank key element 
   >按分数排名
   * zrevrangebyscore key max min 
   >返回有序集中指定分数区间内的成员，分数从高到低排序
   * zrange key start end 
   >返回指定索引范围内的升序元素(分值) O(log(n)+m)
   * zrangebyscore key minScore maxScore 
   >返回指定分数范围内的升序元素 O(log(n)+m)
   * zcount key minScore maxScore 
   >返回有序集合内在指定分数范围内的个数 O(log(n)+m)
   * zremrangebyrank key start end 
   >删除指定排名内的升序元素 O(log(n)+m
   * zremrangebyscore key minScore maxScore 
   >删除指定分数内的升序元素 O(log(n)+m


## 使用场景
   * set 兴趣标签、粉丝集合、共同好友、适合用于聚合分类
   * zset 与排序有关的、榜单、排行榜、最新记录等
   * string 网站访问量、文章浏览量、点赞量、等计数场景(或者普通缓存)
   * hash 商品、用户等基本信息
   * list 消息系统、通知
       - LPUSH + LPOP = Stack（堆栈）
       - LPUSH + RPOP = Queue（队列）
       - LPUSH + LTRIM = Capped Collection（上限集合-环形）
       - LPUSH + BRPOP = Message Queue(消息队列)
       