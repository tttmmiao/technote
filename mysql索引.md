[TOC]

## group by
* Q: using temperary; using filesort和groupby

## 问题
1. 为什么要使用联合索引（多列索引）
     为了减少io操作。
     直接通过遍历索引取得数据，而无需回表，这就减少了很多随机io操作。减少io操作，特别的随机io其实是dba主要的优化策略。所以，在真正的实际应用中，覆盖索引是主要的提升性能的优化手段之一
2. explain的rows是什么意思
预估的需要查询的行数，不等于count(1)得到的结果

3. 
对于范围条件查询，MySQL无法使用范围列后面的其他索引列了，但是对于“多个等值条件查询”则没有这个限制了

4. 关系型数据库： 索引 + 表。  表存储完整记录，有两种组织形式：堆表（所有记录无序存储）和 聚簇索引表（所有记录按照主键进行排序存储）。
5. explain - key_len的计算方法
 * 一般地，key_len 等于索引列类型字节长度，例如int类型为4-bytes，bigint为8-bytes，int(11) 也为4-bytes.
  * 如果是字符串类型，还需要同时考虑字符集因素，例如：CHAR(30) UTF8则key_len至少是90-bytes；
 * 若该列类型定义时允许NULL，其key_len还需要再加 1-bytes；
 * 若该列类型为变长类型，例如 VARCHAR（TEXT\BLOB不允许整列创建索引，如果创建部分索引，也被视为动态列类型），其key_len还需要再加 2-bytes;

6. 若表中索引过多，会影响INSERT及UPDATE性能，简单说就是会影响数据写入性能。因为更新数据的同时，也要同时更新索引。


## 参考：
- [explain_keylen计算方法](http://hedengcheng.com/?p=577
http://www.cnitblog.com/aliyiyi08/archive/2008/09/09/48878.html)

