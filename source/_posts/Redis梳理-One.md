---
title: Redis梳理-One
date: 2019-11-26 21:50:45
tags: [redis, php]
categories:[]
keywords:
toc:
description:"&emsp;&emsp;本篇主要是梳理redis的常用的命令以及数据结构、时间复杂度、使用场景以及一些注意事项，如有瑕疵之处，不吝评论指点。"
---
## redis常用数据结构
***set 集合
***zset 有序集合
***hash 哈希
***list 列表
***string 字符串
## 命令相关
### 1、set集合
特点：无序、无重复、集合间操作

   * sadd key element O(1) 
   * srem key element O(1)
   * sismember 集合中是否存在某个元素
   * srandmember 返回多个元素
   * smembers 无序 小心使用（数量太多不行）scan？
   * spop 从集合中弹出一个
   * sdiff 差集
   * sinter 交集
   * sunion  并集

### 2、zset集合(有序)
特定：无重复元素、有序、element + score
   * zadd key score element 添加 score(可以重复) 和 element（不可以重复） O(logN)
   * zrem key element 删除元素 O(1)
   * zscore key element 返回元素的分数 O(1)
   * zrevrange key start stop返回有序集中指定区间内的成员，通过索引，分数从高到低
   * zrevrank key member 返回有序集中指定区间内的成员，通过索引，分数从高到低
   * zincrby key score 增加或减少元素的分数 O(1)
   * zcard key 返回元素的总个数 O(1)
   * zrank key element 按分数排名
   * zrevrangebyscore key max min 返回有序集中指定分数区间内的成员，分数从高到低排序
   * zrange key start end 返回指定索引范围内的升序元素(分值) O(log(n)+m)
   * zrangebyscore key minScore maxScore 返回指定分数范围内的升序元素 O(log(n)+m)
   * zcount key minScore maxScore 返回有序集合内在指定分数范围内的个数 O(log(n)+m)
   * zremrangebyrank key start end 删除指定排名内的升序元素 O(log(n)+m
   * zremrangebyscore key minScore maxScore 删除指定分数内的升序元素 O(log(n)+m
### 3、hash哈希
特点：Mapmap？、Small redis、field 不能相同，value 可用相同
   * hget key field 获取 hash key 对应的 field 的 value
   * hset key field value 设置 对应的 field 的 value
   * hexists key field 判断 hash key 是否有 field
   * hlen key 获取 hash key field 的数量
   * hmget key field1 field2 ... fieldN 批量获取 hash key 的一批 field 对应的值 O(n)
   * hmset key field1 value1 field2 批量设置 hash key 的一批 field value valueN O(n)
   * hgetall key 返回 hash key 对应所有的 field 和 value O(n)（谨慎使用）
   * havals key 返回 hash key 对应所有的 value O(n)
   * hkeys key 返回 hash key 对应所有的 field O(n)
   * hsetnx key field value 设置 hash key 对应 field 的 value（如 field 已经存在，则失败）
   * hincrby key field intCounter hash key 对应的 field 的 value 自增 intCounter
   * hincrbyfloat key field floatCounter hincrby 浮点数版
### 4、list
特点：有序、重复、左右两边插入弹出
   * push key value1 value2 ... valueN  从列表右端插入值（1-N 个） O(1~n)
   * lpush key value1 value2 ... valueN 从列表左端插入值（1-N 个）O(n)
   * linsert key before|after value newValue 在 list 指定的值前|后插入 newValue
   * lpop key 从列表左侧弹出一个 item O(1)
   * rpop key 从列表右侧弹出一个 item    
   * lrem key count value O(n)
        根据 count 值，从列表中删除所有 value 相等的项 
        count > 0，从左到右，删除最多 count 个 value 相等的项
        count < 0，从右到左，删除最多 Math.abs(count) 个 value 相等的项
        count = 0，删除所有 value 相等的项
   * ltrim key start end  按照索引范围修剪列表 O(n)
   * lrange key start end（包含 end） 获取列表指定索引范围所有 item （获取全部（0 -1））O(n)
   * lindex key index  获取列表指定索引的 item O(n)
