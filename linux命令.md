[TOC]
# linux命令
## awk
* 输出第几列 $0表示整个行
```
awk '{print $1, $2}' xxx.log
```
* 格式化输出
 ```
 awk '{printf "%-8s %-8s %-8s %-18s %-22s %-15s\n",$1,$2,$3,$4,$5,$6}' xxx.txt
 ```
* 按列的值过滤记录

```
awk '$3==0 && $6=="LISTEN" ' xxx.txt
awk ' $3>0 {print $0}' xxx.txt
awk '$3==0 && $6=="LISTEN" || NR==1 ' xxx.txt
  
```

* 指定分隔符过滤数据 -F:

```
 awk  -F: '{print $1,$3,$6}' xxx.log
 awk -F '[;:]' //指定多个分隔符
```

* 输出某一列以后的所有列

```
  awk -F, '{for(i=8;i<=NF;i++) printf"%s ",$i} {print ""}' xxx.log
```

[酷壳-参考](https://coolshell.cn/articles/9070.html)

## head
* 第一次出现的行
```
grep -n "string" xxx.log | head -n 1
```

## tail
删除前3行
```
tail -n +3 origin.log > target.log
```
## sort uniq

```
sort xxx.log | uniq
sort xxx.log | uniq -u //output the unique context
sort xxx.log | uniq -d // output the duplicate context
```


## sed
* d命令 删除匹配行
* 
```
 sed '/abc/d;/efg/d' a.txt > a.log    // 删除含字符串"abc"或“efg"的行，将结果保存到a.log
 
 若报错sed: RE error: illegal byte sequence，在当前目录下执行两条语句 export LC_COLLATE='C'
export LC_CTYPE='C'
```
[sed tutorial](https://coolshell.cn/articles/9104.html)

##  scp

* 复制目录到远程
    scp -r local_folder remote_ip:remote_folder
* 远程文件到本地
    scp xxx.log user@ip:/folder/

## mv
   重命名文件
   mv source.txt target.txt

##  man xx

查询xx的作用，j,k为向上向下加载

## less xx

以流的方式输出xx的内容

## more xx
逐步输出文件内容

## cat xx

完全输出xx的内容

## ps

process status

常用 ps -ef

对应杀掉进程使用kill -9 进程号

## grep xxx

搜索xxx，常用的查看日志的方法：

grep 关键词文件名
zgrep 查看解压文件

##  |的作用

管道

## df
output the disk info

## du 
磁盘所占空间

## su
切换用户
e.g., su - work

## ps -ef | grep java
查看java进程

## others

**查看占用的进程**：
如  lsof -i tcp:1099
杀掉该进程 kill 进程号
[查看进程占用情况](http://www.jianshu.com/p/8d167e3bca50)

**清理/data中大内存文件内容**
 find /data -size +800M | xargs rm 
 
 * 过滤日志条数 grep '关键字' xxx.log | wc -l
 
 * nginx日志分析
 
 * 启动Tomcat  sudo /etc/init.d/tomcat forcestart
 * 停止Tomcat sudo /etc/init.d/tomcat forcestop
 
 