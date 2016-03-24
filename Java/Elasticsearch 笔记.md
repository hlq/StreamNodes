# Elasticsearch 笔记
>  建议使用ppa安装最新jdk

## 添加apt
```bash
#添加GPG KEY
wget -qO - https://packages.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
#添加源
echo "deb http://packages.elastic.co/elasticsearch/2.x/debian stable main" | sudo tee -a /etc/apt/sources.list.d/elasticsearch-2.x.list
```
## [安装](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html)

```bash
sudo apt-get update && sudo apt-get install elasticsearch
```
- 自启动
```bash
sudo update-rc.d elasticsearch defaults 95 10
```
- [远程访问](https://www.elastic.co/guide/en/elasticsearch/reference/2.2/modules-network.html)
```bash
vi /etc/elasticsearch/elasticsearch.yml
#修改
network.host: 0.0.0.0
```
- 启动
```bash
/etc/init.d/elasticsearch start
```
> 安装成功访问： http://133.130.116.206:9200/

##[Kibana安装](https://www.elastic.co/guide/en/kibana/current/setup.html)
- apt-get
```bash
#key
wget -qO - https://packages.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
#添加源
echo "deb http://packages.elastic.co/kibana/4.4/debian stable main" | sudo tee -a /etc/apt/sources.list
#安装
sudo apt-get update && sudo apt-get install kibana

#自启动
sudo update-rc.d kibana defaults 95 10
```
> 安装成功访问： http://133.130.116.206:5601

## [Marvel安装](https://www.elastic.co/guide/en/marvel/current/index.html)

- 安装elasticsearch下插件
```bash
#/usr/share/elasticsearch/
bin/plugin install license
bin/plugin install marvel-agent
```
- 安装kibana下插件
```bash
#/opt/kibana/
bin/kibana plugin --install elasticsearch/marvel/latest
```

## ElasticSearch各个目录说明



|type	|description	|location|
| :-------- | --------:| :--: |
|home	|Home of elasticsearch installation	|/usr/share/elasticsearch|
|bin	|Binary scripts including elasticsearch to start a node	|/usr/share/elasticsearch/bin|
|conf|	Configuration files elasticsearch.yml and logging.yml	|/etc/elasticsearch|
|conf|	Environment variables including heap size,file descriptors	|/etc/default/elasticsearch|
|data|	The location of the data files	|/var/lib/elasticsearch/|
|logs|	Log files location	|/var/log/elasticsearch|
|plugins	|Plugin files location	|/usr/share/elasticsearch/plugins|


## [配置](https://www.elastic.co/guide/en/elasticsearch/guide/current/index.html)

- ES_HEAP_SIZE
```bash
# 在/etc/default/elasticsearch中修改：
ES_HEAP_SIZE=4g    #不要超过32g，如果整台机器只部署ES，一半内存用于Java heap，另一半给Lucene
```

- File Descriptors
```bash
cat <<EOF>> /etc/security/limits.conf
elasticsearch - nofile 65535
EOF

# 在/etc/default/elasticsearch中修改：
MAX_OPEN_FILES=65535
```

- Virtual memory
```bash
cat <<EOF>> /etc/sysctl.conf
vm.max_map_count=262144
EOF

sysctl -p
```

- Memory Settings
```bash
# 在/etc/elasticsearch/elasticsearch.yml中修改：
bootstrap.mlockall: true
# 在/etc/default/elasticsearch中修改：
MAX_LOCKED_MEMORY=unlimited
```

- 其他
```bash
在/etc/elasticsearch/elasticsearch.yml中修改：

# 集群名称，同一集群，名称要设置相同
cluster.name: elasticsearch_production
# 节点名称
node.name: elasticsearch_001_data

# 数据路径，可配置多个，英文逗号分开，注意目录的权限，保证elasticsearch用户可写
path.data: /path/to/data1,/path/to/data2 
# 日志路径，注意目录的权限，保证elasticsearch用户可写
path.logs: /path/to/logs
# 插件路径
path.plugins: /path/to/plugins

# 该属性是为了形成一个集群，有主节点资格并互相连接的节点的最小数目
# (number of master-eligible nodes / 2) + 1。 下面的值是在3个有主节点资格的情况下设定
# 因为节点数，以后可以增加，或者减少，故该配置可以动态修改
discovery.zen.minimum_master_nodes: 2

# 恢复控制
gateway.recover_after_nodes: 2
gateway.expected_nodes: 3
gateway.recover_after_time: 5m

#关闭多播，用单播。并指定至少一个能接受单播的主机
discovery.zen.ping.multicast.enabled: false 
discovery.zen.ping.unicast.hosts: ["192.168.2.1:9300", "192.168.2.2:9300", "192.168.2.3:9300"]

```

