---
title: 'MySQL之集合之间的查询'
date: 2021-01-27 19:38:07
tags:
 - Spring
 - MySql
categories:
 - MySql
---



**此为Spring Mapping层的策略**
根据需求有以下查询情况：

 1. 判断一个元素是否在集合里面。
 可用 IN 判断，要求传入参数为集合，然后通过

```xml
 <foreach collection="List" item="i" index="index" open="(" close=")" separator=",">
 </foreach>
```
对集合进行整理。
另一个方法是使用 FIND_IN_SET 函数，这个要求传入参数为以逗号分隔的字符串。
如 FIND_IN_SET(em,emlist) , FIND_IN_SET('1','1,2,3' ) 会回传元素所在索引。不存在返回0。

 2. 判断一个集合里面是否有另一个集合的元素。
 使用策略也可以传列表操作，使用上面foreach进行操作每一个元素是否在另一个。
 另外就是使用正则 
```sql
select concat('1,4', ',') regexp concat(replace('1,2,3',',',',|'),',') -- 1
select concat('5,4', ',') regexp concat(replace('1,2,3',',',',|'),',') -- 0
```
前面的存在返回1，不存在返回0；需要注意的是，这个方法如果存在前面的集合元素包含后面的集合元素时候，也会返回1，所以可以在前后加上不可能出现的字符来判断。


