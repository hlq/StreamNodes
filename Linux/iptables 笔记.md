#iptables 笔记
## 命令
```bash
#查看策略列表
iptables -L -n 
#禁止IP
iptables -I INPUT -s 10.10.10.10 -j DROP
#删除禁止IP
iptables -D INPUT -s 10.10.10.10 -j DROP
#删除所有策略
iptables -F
```

## 备注
```bash
#Ubuntu iptables位置 在shell里调用
/sbin/iptables 
```
