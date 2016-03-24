# elasticsearch 配置详解

elasticsearch的config文件夹里面有两个配置文件：elasticsearch.yml和logging.yml，第一个是es的基本配置文件，第二个是日志配置文件，es也是使用log4j来记录日志的，所以logging.yml里的设置按普通log4j配置文件来设置就行了。下面主要讲解下elasticsearch.yml这个文件中可配置的东西。 

配置es的集群名称，默认是elasticsearch，es会自动发现在同一网段下的es，如果在同一网段下有多个集群，就可以用这个属性来区分不同的集群。 
```bash
cluster.name: elasticsearch  
```

节点名，默认随机指定一个name列表中名字，该列表在es的jar包中config文件夹里name.txt文件中，其中有很多作者添加的有趣名字。 
```bash
node.name: "Franz Kafka"  
```

指定该节点是否有资格被选举成为node，默认是true，es是默认集群中的第一台机器为master，如果这台机挂了就会重新选举master。 
```bash
node.master: true  
```

指定该节点是否存储索引数据，默认为true。 
```bash
node.data: true  
```

设置默认索引分片个数，默认为5片。 
```bash
index.number_of_shards: 5  
```

设置默认索引副本个数，默认为1个副本。 
```bash
index.number_of_replicas: 1  
```

设置配置文件的存储路径，默认是es根目录下的config文件夹。 
```bash
path.conf: /path/to/conf  
```

设置索引数据的存储路径，默认是es根目录下的data文件夹 
```bash
path.data: /path/to/data  
```

可以设置多个存储路径，用逗号隔开，例： 
```bash
path.data: /path/to/data1,/path/to/data2  
```

设置临时文件的存储路径，默认是es根目录下的work文件夹。 
```bash
path.work: /path/to/work  
```

设置日志文件的存储路径，默认是es根目录下的logs文件夹 
```bash
path.logs: /path/to/logs  
```

设置插件的存放路径，默认是es根目录下的plugins文件夹 
```bash
path.plugins: /path/to/plugins  
```

强制所有内存锁定，不要搞什么swap的来影响性能 
设置为true来锁住内存。因为当jvm开始swapping时es的效率会降低，所以要保证它不swap，可以把ES_MIN_MEM和ES_MAX_MEM两个环境变量设置成同一个值，并且保证机器有足够的内存分配给es。同时也要允许elasticsearch的进程可以锁住内存，linux下可以通过`ulimit -l unlimited`命令。 
```bash
bootstrap.mlockall: true  
```

设置绑定的ip地址，可以是ipv4或ipv6的，默认为0.0.0.0。 
```bash
network.bind_host: 192.168.0.1  
```

设置其它节点和该节点交互的ip地址，如果不设置它会自动判断，值必须是个真实的ip地址。 
```bash
network.publish_host: 192.168.0.1  
```

这个参数是用来同时设置bind_host和publish_host上面两个参数。 
```bash
network.host: 192.168.0.1  
```

设置节点间交互的tcp端口，默认是9300。 
```bash
transport.tcp.port: 9300  
```

设置是否压缩tcp传输时的数据，默认为false，不压缩。 
```bash
transport.tcp.compress: true  
```

设置对外服务的http端口，默认为9200。 
```bash
http.port: 9200  
```

设置内容的最大容量，默认100mb 
```bash
http.max_content_length: 100mb  
```

是否使用http协议对外提供服务，默认为true，开启。 
```bash
http.enabled: false  
```

网络配置 
```bash
#network.tcp.keep_alive : true  
#network.tcp.send_buffer_size : 8192  
#network.tcp.receive_buffer_size : 8192  
```

自动发现相关配置 
```bash
#discovery.zen.fd.connect_on_network_disconnect : true  
#discovery.zen.initial_ping_timeout : 10s  
#discovery.zen.fd.ping_interval : 2s  
#discovery.zen.fd.ping_retries  : 10  
```

The gateway snapshot interval (only applies to shared gateways). 
```bash
#index.gateway.snapshot_interval : 1s  
```
分片异步刷新时间间隔 
```bash
#index.refresh_interval : -1  
```

Set to an actual value (like 0-all) or false to disable it. 
```bash
index.auto_expand_replicas  
```

Set to true to have the index read only. false to allow writes and metadata changes. 
```bash
index.blocks.read_only  
```

Set to true to disable read operations against the index. 
```bash
index.blocks.read  
```

Set to true to disable write operations against the index. 
```bash
index.blocks.write  
```

Set to true to disable metadata operations against the index. 
```bash
index.blocks.metadata  
```

Lucene index term间隔，仅用于新创建的doc 
```bash
index.term_index_interval  
```

Lucene reader term index divisor 
```bash
index.term_index_divisor  
```

When to flush based on operations. 
```bash
index.translog.flush_threshold_ops  
```

When to flush based on translog (bytes) size. 
```bash
index.translog.flush_threshold_size  
```

When to flush based on a period of not flushing. 
```bash
index.translog.flush_threshold_period  
```

Disables flushing. Note, should be set for a short interval and then enabled. 
```bash
index.translog.disable_flush  
```

The maximum size of filter cache (per segment in shard). Set to -1 to disable. 
```bash
index.cache.filter.max_size  
```

The expire after access time for filter cache. Set to -1 to disable. 
```bash
index.cache.filter.expire  
```

merge policy 
All the settings for the merge policy currently configured. A different merge policy can’t be set. 

A node matching any rule will be allowed to host shards from the index. 
```bash
index.routing.allocation.include.*  
```

A node matching any rule will NOT be allowed to host shards from the index.	
```bash
index.routing.allocation.exclude.*  
```

Only nodes matching all rules will be allowed to host shards from the index.	
```bash
index.routing.allocation.require.*  
```

Controls the total number of shards allowed to be allocated on a single node. Defaults to unbounded (-1). 
```bash
index.routing.allocation.total_shards_per_node  
```

When using local gateway a particular shard is recovered only if there can be allocated quorum shards in the cluster. It can be set to quorum (default), quorum-1 (or half), full and full-1. Number values are also supported, e.g. 1. 
```bash
index.recovery.initial_shards  
```

Disables temporarily the purge of expired docs. 
```bash
index.ttl.disable_purge  
```

默认索引合并因子 
```bash
#index.merge.policy.merge_factor : 100  
#index.merge.policy.min_merge_docs : 1000  
#index.merge.policy.use_compound_file : true  
#indices.memory.index_buffer_size : 5%  
```

Gateway相关配置 
当集群期望节点达不到的时候，集群就会处于block，无法正常索引和查询，说明集群中某个节点未能正常启动，这正是我们期望的效果，block住，避免照成数据的不一致。 
gateway的类型，默认为local即为本地文件系统，可以设置为本地文件系统，分布式文件系统，hadoop的HDFS，和amazon的s3服务器，其它文件系统的设置方法下次再详细说。 
```bash
gateway.type: local  
```

设置集群中N个节点启动时进行数据恢复，默认为1。 
```bash
gateway.recover_after_nodes: 1  
```

设置初始化数据恢复进程的超时时间，默认是5分钟。 
```bash
gateway.recover_after_time: 5m  
```

设置这个集群中节点的数量，默认为2，一旦这N个节点启动，就会立即进行数据恢复。 
```bash
gateway.expected_nodes: 2  
```

初始化数据恢复时，并发恢复线程的个数，默认为4。 
```bash
cluster.routing.allocation.node_initial_primaries_recoveries: 4  
```

添加删除节点或负载均衡时并发恢复线程的个数，默认为4。 
```bash
cluster.routing.allocation.node_concurrent_recoveries: 2  
```

设置数据恢复时限制的带宽，如入100mb，默认为0，即无限制。 
```bash
indices.recovery.max_size_per_sec: 0  
```

设置这个参数来限制从其它分片恢复数据时最大同时打开并发流的个数，默认为5。 
```bash
indices.recovery.concurrent_streams: 5  
```

设置这个参数来保证集群中的节点可以知道其它N个有master资格的节点。默认为1，对于大的集群来说，可以设置大一点的值（2-4）。 
```bash
discovery.zen.minimum_master_nodes: 1  
```

设置集群中自动发现其它节点时ping连接超时时间，默认为3秒，对于比较差的网络环境可以高点的值来防止自动发现时出错。 
```bash
discovery.zen.ping.timeout: 3s  
```

```bash
discovery.zen.ping.multicast.enabled: false  
```
设置是否打开多播发现节点，默认是true。 
当禁用multcast广播的时候，可以手动设置集群的节点ip 

设置集群中master节点的初始列表，可以通过这些节点来自动发现新加入集群的节点。 
```bash
discovery.zen.ping.unicast.hosts: ["host1", "host2:port", "host3[portX-portY]"]  
```

下面是一些查询时的慢日志参数设置 
```bash
index.search.slowlog.level: TRACE  
index.search.slowlog.threshold.query.warn: 10s  
index.search.slowlog.threshold.query.info: 5s  
index.search.slowlog.threshold.query.debug: 2s  
index.search.slowlog.threshold.query.trace: 500ms  
  
index.search.slowlog.threshold.fetch.warn: 1s  
index.search.slowlog.threshold.fetch.info: 800ms  
index.search.slowlog.threshold.fetch.debug:500ms  
index.search.slowlog.threshold.fetch.trace: 200ms  
```


1.设置cache大小和过期时间。 
  
```bash
index.cache.field.max_size  
index.cache.field.expire  
```

例如设置： 
  //index中每个segment中可包含的最大的entries数目 
 
```bash
index.cache.field.max_size: 50000   
```
  //过期时间为10分钟 
 
```bash
index.cache.field.expire: 10m   
```

2.改变cache类型。 
```bash
index.cache.field.type: soft  
```
默认类型为resident， 字面意思是常驻（居民）， 一直增加，直到内存 耗尽。 改为soft就是当内存不足的时候，先clear掉 占用的，然后再往内存中放。设置为soft后，相当于设置成了相对的内存大小。resident的话，除非内存够大。 

3.对数据进行处理。 
文章中提到的是减小字段值长度，如将大写转成小写。 
这点上，实际中可能将数据精炼。当然， 也可以把要做facet的字段做一个转化，用int型代替。 
关于string转化int呢， 可以参考M大神的: https://github.com/medcl/elasticsearch-analysis-string2int

