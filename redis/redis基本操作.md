# redis基本操作

```bash
##################安装#####################
apt-get install redis-server      #Ubuntu 安装
ps -aux|grep redis                #进程检测
netstat -nlt|grep 6379            #端口


##################配置#####################
vi /etc/redis/redis.conf          #编辑redis.conf配置文件
requirepass ******                #编辑redis登录密码
#bind 127.0.0.1                   #注释bind 开启远程登录redis
/etc/init.d/redis-server restart     #重启redis
/etc/init.d/redis-server status   #通过启动命令检查Redis服务器状态

##################登陆#####################
redis-cli                         #进入redis控制台
redis-cli -a ****                 #使用密码登录

##################操作#####################
keys *                            #查看所有key列表
set key1 "hello"                  #添加一条key=key1 value=hello记录
get key1                          #获取key=key1 的记录
INCR key2                         #key2自增长+1 1 -> 2
LPUSH key3 a                      #列表key3 左侧增加记录a
RPUSH key3 b                      #列表key3 右侧增加记录b
LRANGE key3 0 3                   #从左向右打印列表key
HSET key4 name "Stream"
HSET key4 email "javaeer@qq.com"  #哈希表key4 中添加key和value
HGET key4 name                    #获取哈希表key4中key=name对应的value
HGETALL key4                      #输出整改哈希表key4
HMSET key5 username Stream password 123456 age 3 #一次向哈希表key5插入多条数据
HMGET key5 username password      #一次从哈希表key5中读取username password
HGETALL key5                      #输出哈希表key5所以数据库
del key1                          #删除key1
```

