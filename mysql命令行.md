
[TOC]
# mysql索引
## index

* 新建索引 create index index_name on table_name (column1, column2, ……)
* 新建唯一索引 create unique index index_name on table_name (column1, column2, ……)
* 删除索引 drop index index_name on table_name
* 查看索引 show index from table_name ;  show keys from table_name

## key



## key和index的区别

key 是关系模型理论中的一部分，用于数据完整性与唯一性约束等。

index， 可以对表的任意列建立索引，当建立索引的列处于sql的where条件中时，可得到快速的数据定位进而快速检索。

## explain 检测sql 性能 

explain返回的信息有

| 名称            | 含义           | 备注                                       |
| ------------- | ------------ | ---------------------------------------- |
| select_type   |              |                                          |
| type          | 表示连接使用了何种类型  | const > eq_reg > ref > range > index > all |
| possible_keys | 可能用在这张表中的索引  |                                          |
| key           | 实际使用的索引      |                                          |
| key_len       | 使用索引的长度      | 在不损失精确性的情况下，长度越短越好                       |
| ref           | 显示索引的哪一列被使用了 |                                          |
| extra_info    | 额外信息         |                                          |



## 不走索引的情况

* 索引列参与了计算 。 如age为索引列，where age +10 = 20不走索引

* 索引列使用了函数。

* 索引列使用了Like ％xxx. 前缀xxx%走索引，后缀%xxx不走索引，所以搜索email列中.com结尾的字符串而email上希望走索引时候,可以考虑数据库存储一个反向的内容reverse_email。


# binlog
* binary log file
* writes updates and changes as 'events' to the binary log
* cannot configure the master to log only certain events.
* [replication](https://dev.mysql.com/doc/refman/5.7/en/replication.html)
## configure
* show variables like '%log_bin%'
  if = OFF, then ref [enable log_bin](https://dev.mysql.com/doc/refman/5.7/en/replication-howto-masterbaseconfig.html)
  
## related parms

```
   show variables like '%log_bin%'

```
* log-bin: assign log file
* log_bin_index: the position of index file
* binlog_cache_size: how many transactions cache in the memory
* sync_binlog: x means flush the data to dish after the data in the cache commit x times 

```
show binary logs 
```
list the binary logs 



# mysql命令行
## log in

 ``` 
 mysql -u user_name -h  example.mysql.alibabalabs.com -P3306 -pxxxx
 ```
 -u 指定的是用户名， -h指定的是主机名， -P指定的是端口， -p指定的是密码。

## load

```
  mysql -h%s -u%s -p%s -P%d  %s -f <  %s" 
  % (Host, User, Passwd, Port, DB,import_file)
```
 或者
  source xxx.sql
 
## mysqldump
数据库备份-simple

 ```
 mysqldump -u root 库名>xxx.data
 ```
 
 备份数据库命令
 ```
 /home/mysql/mysql/bin/mysqldump -uroot --single-transaction --skip-add-drop-table --master-data=2 --set-gtid-purged=off 库名 > `dirname $0`/库名_`date +   %Y%m%d%H%M%S`.sql
 ```
 
* 查看数据库版本
  select @@version; 或者 status;
  
  
## mysqlslap压测
* 连接

```
mysqlslap -u s_miao -h mysql-cen-test-cloud-ssd-1.db.beta.alsh.xingbianli.com -P3306 -p2378666

```

* 查询 50，100个并发线程，运行1000个查询

```
mysqlslap -a --concurrency=50,100 --number-of-queries=1000 -u s_miao -h mysql-cen-test-cloud-ssd-1.db.beta.alsh.xingbianli.com -P 3306 -p2378666
```

* 1,10,50,100个并发线程，运行200个查询，迭代10次，自动生成sql测试脚本，读、写、更新混合测试、自增长字段、测试引擎为innodb

```
mysqlslap -us_miao -h mysql-cen-test-cloud-ssd-1.db.beta.alsh.xingbianli.com -p2378666 --concurrency=50,100,200 --number-of-queries=200 --iterations=10 --auto-generate-sql --auto-generate-sql-load-type=mixed --auto-generate-sql-add-autoincrement --engine=innodb
```

* 查看详情使用-a --only-print
--only-print	Do not connect to databases. mysqlslap only prints what it would have done

```
mysqlslap -us_miao -h mysql-cen-test-cloud-ssd-1.db.beta.alsh.xingbianli.com -p2378666 --concurrency=50 --number-of-queries=200 --iterations=10 --auto-generate-sql --auto-generate-sql-load-type=mixed --auto-generate-sql-add-autoincrement --engine=innodb -a --only-print
```

* 使用1个索引
```
  mysqlslap -us_miao -h mysql-cen-test-cloud-ssd-1.db.beta.alsh.xingbianli.com -p2378666 --concurrency=50,100,200 --number-of-queries=200 --iterations=10 --auto-generate-sql --auto-generate-sql-load-type=mixed --auto-generate-sql-add-autoincrement --engine=innodb --auto-generate-sql-secondary-indexes=1
```
  
## mysql 压力测试
 
```
- cpu
sysbench --test=cpu --cpu-max-prime=2000 run
- 线程
sysbench --test=threads --num-threads=500 --thread-yields=100 --thread-locks=4 run
- IO性能
1，prepare阶段，生成需要的测试文件，完成后会在当前目录下生成很多小文件。
sysbench --test=fileio --num-threads=16 --file-total-size=2G --file-test-mode=rndrw prepare
sysbench --test=fileio --num-threads=16 --file-total-size=2G --file-block-size=4K --file-test-mode=rndrw prepare
2，run阶段
sysbench --test=fileio --num-threads=20 --file-total-size=2G --file-test-mode=rndrw run
sysbench --test=fileio --num-threads=20 --file-total-size=2G --file-block-size=4K --file-test-mode=rndrw run
 
while true; do \
sysbench --test=fileio --num-threads=20 --file-total-size=2G --file-test-mode=rndwr --file-block-size=4K --file-fsync-all=on --percentile=99 run ;\
done
 
sysbench --test=fileio --num-threads=20 --file-total-size=2G --file-test-mode=seqwr --file-fsync-all=on --percentile=99 run
3，清理测试时生成的文件
sysbench --test=fileio --num-threads=20 --file-total-size=2G --file-test-mode=rndrw cleanup
- 内存性能
sysbench --test=memory --memory-block-size=8k --memory-total-size=1G run
- 互斥量
sysbench –test=mutex –num-threads=100 –mutex-num=1000 –mutex-locks=100000 –mutex-loops=10000 run
```
 
### mysqlslap参数设置
```
mysqlslap --concurrency=1000 --iterations=1 \
--number-int-cols=10 --number-char-cols=20 \
--auto-generate-sql --number-of-queries=1000000 \
--auto-generate-sql-load-type=write \
--auto-generate-sql-execute-number=10000 --auto-generate-sql-unique-query-number=10000
```
 
### 内存计算
```
select
(@@key_buffer_size + @@query_cache_size + @@tmp_table_size
+ @@innodb_buffer_pool_size
+ @@innodb_log_buffer_size
+ @@max_connections * (
@@read_buffer_size + @@read_rnd_buffer_size
+ @@sort_buffer_size+ @@join_buffer_size
+ @@binlog_cache_size + @@thread_stack
)
)/1024/1024/1024;
```

## sysbench压测
[安装方法](http://keithlan.github.io/2016/12/16/sysbench_mysql/)
### 测试步骤
prepare(准备数据) -> run(运行测试) -> cleanup(清理数据)
### 结果处理
QPS = 总select数量 / 测试总时长(秒)

### 测试过程
#### cpu测试
```
sysbench --mysql-host=mysql-cen-test-cloud-ssd-1.db.beta.alsh.xingbianli.com --mysql-user=s_miao--mysql-password=2378666 --test=cpu --cpu-max-prime=20000 run
```
#### mysql-oltp测试

```
指定数据库建了10张表，每张表数量为1w
sysbench /usr/share/sysbench/oltp_read_only.lua \
    --db-driver=mysql \
    --mysql-host=mysql-cen-test-cloud-ssd-1.db.beta.alsh.xingbianli.com --mysql-port=3306 --mysql-user=s_miao --mysql-password=2378666 \
    --mysql-db=press_test --tables=10 --table-size=10000 \
    --report-interval=10 \
    --threads=128 --time=120 \
    prepare/run/cleanup
```
* lua脚本说明
  - oltp_read_only.lua

  
## 其他
* sql优化
  [sql优化1](http://www.cnblogs.com/zhiqian-ali/p/6336521.html)