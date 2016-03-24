# rabbitmq 安装和配置


----------------
[TOC]


##[安装](http://rabbitmq-into-chinese.readthedocs.org/)
- **配置apt源**
将以下的行添加到你的 /etc/apt/sources.list 文件中：
```bash
deb http://www.rabbitmq.com/debian/ testing main
```
*（可选的）为了避免未签名的错误信息，请使用apt-key(8)命令将我们的公钥添加到你的可信任密钥列表中：*
```bash
wget http://www.rabbitmq.com/rabbitmq-signing-key-public.asc 
sudo apt-key add rabbitmq-signing-key-public.asc
```
- **更新安装**
```bash
apt-get update
sudo apt-get install rabbitmq-server
```

> [rabbitmq入门教程](http://www.tuicool.com/articles/JFvARbb)



------

##[RabbitMQ的远程Web管理与监控工具](http://www.open-open.com/lib/view/open1432468144338.html)

rabbitmq-management plugin提供HTTP API来管理和监控RabbitMQ Server，具体包含如下功能：
- 删除、生成、列表，包括：exchanges，queues，bindings，users，virtual hosts and permissions。

- 监视 queue 长度，每个 channel的message rates ，每个连接的data rates，等等。

- 发送和接收messages。

- 监控Erlang processes，file descriptors，memory use。

- 导出／导出object definitions to JSON。

- 强制关闭 connections，清空 queues。

## 添加远程管理账户

如果要从远程登录怎么做呢？处于安全考虑，guest这个默认的用户只能通过http://localhost:15672来登录，其他的IP无法直接用这个guest帐号。这里我们可以通过配置文件来实现从远程登录管理界面，只要编辑/etc/rabbitmq/rabbitmq.config文件（没有就新增），添加以下配置就可以了。

```bash
[{rabbit, [{tcp_listeners, [5672]}, {loopback_users, ["stream"]}]}].
```
授权用户stream
```bash
$  cd /usr/lib/rabbitmq/bin/
#用户名与密码
$ sudo rabbitmqctl add_user stream xxxx
#用户设置为administrator才能远程访问
$ sudo rabbitmqctl set_user_tags asdf administrator         
$ sudo rabbitmqctl set_permissions -p / asdf ".*" ".*" ".*"
```


