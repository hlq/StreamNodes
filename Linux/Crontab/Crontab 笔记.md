#Crontab 笔记

## 任务
```bash
#创建命令
crontab -e
#编写任务
# /etc/profile添加环境变量;>/opt/cron.log 将错误日志
*/2 * * * * . /etc/profile;/opt/Log.sh > /opt/cron.log 2>&1
```
![Alt text](./1458786135574.png)

> [环境变量相关](http://blog.csdn.net/dancen/article/details/24355287)
## 日志
```bash
vi /etc/rsyslog.d/50-default.conf 
#开启注释
cron.*   /var/log/cron.log 
#重启日志系统
service rsyslog restart
#重启crontab
service cron restart
```

## iptables自动封连接数较大的IP防CC
- 未测试供参考

```bash
#!/bin/bash
#Created by http://www.myhack58.com
num=100 #上限
list=`netstat -an |grep ^tcp.*:80|egrep -v 'LISTEN|127.0.0.1'|awk -F"[ ]+|[:]" '{print $6}'|sort|uniq -c|sort -rn|awk '{if ($1>$num){print $2}}'`
for i in $list
do
	iptables -I INPUT -s $i --dport 80 -j DROP
done

crontab -e
*/5 * * * * sh /path/file.sh #5分钟执行一次
```
