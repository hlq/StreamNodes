# rabbitmq 安装和配置


----------------
[TOC]


##[安装](http://rabbitmq-into-chinese.readthedocs.org/)
- **配置apt源**
将以下的行添加到你的 /etc/apt/sources.list 文件中：
``` 
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


