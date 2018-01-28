[TOC]
# linux命令

##  scp

* 复制目录到远程
    scp -r local_folder remote_ip:remote_folder

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

##  |的作用

管道

## df -u

## du -sh


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
 
 