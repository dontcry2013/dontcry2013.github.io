---
title: Linux Commands
date: 2016-04-12 09:47:11
categories: Linux
tags: [Command]
---
一些常用的Linux命令
<!--more-->
grep选项
	-c 只输出匹配行的计数
    -i 不区分大小写（用于单字符）
    -n 显示匹配的行号
    -v 不显示不包含匹配文本的所以有行
    -s 不显示错误信息
    -E 使用扩展正则表达式
``` bash
top    
ps -ef    
ps aux

df -lh
du -sh 
ls -lh

tail -f catalina.out 
tail -n20 /var/log/mail/info |tee results.txt |head -n1  #选择最后 20 行，将其保存到 results.txt，但是只在屏幕上显示这 20 行中的第一行
#tee 命令有一个非常有用的选项(-a)，它允许您将数据追加到已有文件。

cat -n #打印行号
cat > filename  #创建文件
cat pushcourier.log.2015-04-20|grep DIANHUA|wc -l

cat api.log | grep -inE --color=auto "优速速递|doMessage"
cat catalina.out | grep -C 5 method_saveavatar  #前后五行

cat /etc/passwd | sort -t':' -k 7 -u  #第七个域进行排序，然后去重
cat logsn_2015-11-22.txt | grep saveCookie |awk -F'>' '{print $4}' | sort -u | wc -l
cat logsn_2015-11-22.txt | grep insertOrder | grep true | awk -F'>' '{print $3}' | sort | uniq -c |wc 

cat search.log | grep poll | awk -F[\|] '{print $5"#"$6}'| sort | uniq -c | sort -n
cat api.log.150706 | grep '|poll|' | awk -F[\|] '{print $5","$6}' | sort | uniq -c | wc -l 
cat search.log.150708 | grep '|query|' | awk -F[\|] '{print $5","$6}' | sort | uniq -c | wc -l
cat search.log.150708 | awk -F[\|] '{print $5","$6}' | sort | uniq -c | wc -l

iostat -xm 2
free -m
find ~/ -name node  
find -name 'logjd_*' | xargs rm -f

netstat -lnap | grep 1134

curl -H 'X-Real-IP:192.168.0.1' -H 'X-Forwarded-For:192.168.0.2' -v 'http://192.168.248.201:9101/query?type=yuantong&postid=888888888'
wget

grep 'jin' a.txt |grep 'qi'
grep -n "A\|B" *

uptime
```

# vi命令

``` bash
:set nu    #设置显示行号  
:set number  #设置显示行号  
:0		#文件首
:$		#文件尾        
dd     	#删除行        
u  		#撤销改变
w   	#向后移动一个单词
/abc<Enter>      #查找abc, 输入n字符查找下一个。
```

# java调试过程

1、ps -ef | grep kd_mobile | grep -v grep
2、找出该进程内最耗费CPU的线程，可以使用ps -Lfp pid或者ps -mp pid -o THREAD, tid, time或者top -Hp pid
3、printf "%x\n" 24730
4、jstack 24715 | grep 609a

# node命令

``` bash
# 重启node服务
ssh kuaidi@192.168.248.203 -p2222
kill -s 9 1772
nohup node loadjd.js &

# forever命令
mv macloadjd.js loadjd.js
forever stop app.js 
forever start -e ./log/jdforever.error.log -o out.log -a loadjd.js
./node /home/kuaidi/node_modules/forever/bin/forever start -o /home/kuaidi/out.log -e /home/kuaidi/err.log -a /home/kuaidi/proxyclientadsl.js

# 配置临时环境变量
export PATH=/opt/kuaidi100/node/node-v0.10.28-linux-x64/bin:$PATH
```

# 配置防火墙
iptables
iptables -t mangle -F
vi /etc/sysctl.conf
#半连接攻击 syn flood
#启用这个在高并发的时候，部分连接会丢失，确保端口可以打开
#编辑 net.ipv4.tcp_syncookies = 1

iptables -I INPUT -s 42.62.37.0/24 -j DROP

＃ 常用解压缩命令
.tar
解包：tar xvf FileName.tar
打包：tar cvf FileName.tar DirName
（注：tar是打包，不是压缩！）
———————————————
.gz
解压1：gunzip FileName.gz
解压2：gzip -d FileName.gz
压缩：gzip FileName

.tar.gz 和 .tgz
解压：tar zxvf FileName.tar.gz
压缩：tar zcvf FileName.tar.gz DirName
———————————————
.bz2
解压1：bzip2 -d FileName.bz2
解压2：bunzip2 FileName.bz2
压缩： bzip2 -z FileName

.tar.bz2
解压：tar jxvf FileName.tar.bz2
压缩：tar jcvf FileName.tar.bz2 DirName
———————————————
.bz
解压1：bzip2 -d FileName.bz
解压2：bunzip2 FileName.bz
压缩：未知

.tar.bz
解压：tar jxvf FileName.tar.bz
压缩：未知
———————————————
.zip
解压：unzip FileName.zip
压缩：zip FileName.zip DirName
———————————————
.rar
解压：rar x FileName.rar
压缩：rar a FileName.rar DirName
安装：sudo brew install unrar
