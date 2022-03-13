# 一、linux-cenos7-arrch64问题

- 问题起因开放9200、9300端口后外网无法访问

- 关闭防火墙后依旧无法访问

- 修改 elasticsearch.yml 文件配置

  ```shell
  network.host: 10.0.0.30
  ```

  启动报了一堆错误：

  ```shell
  [1]: max file descriptors [4096] for elasticsearch process is too low, increase to at least [65535]
  
  #解决方式 :
  vim /etc/security/limits.conf
  #增加配置
  * soft nofile 65535
  * hard nofile 65535
  
  [2]: max number of threads [3818] for user [admin] is too low, increase to at least [4096]
  #解决方式
  vim /etc/security/limits.conf
  #增加配置
  * soft nproc  4096
  * hard nproc  4096
  
  [3]: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]
  #解决方式
  vim /etc/sysctl.conf
  #增加配置
  vm.max_map_count=262144 
  
  [4]: the default discovery settings are unsuitable for production use; at least one of [discovery.seed_hosts, discovery.seed_providers, cluster.initial_master_nodes] must be configured
  #解决方式
  vim elasticsearch.yml 
  #增加配置
  cluster.initial_master_nodes: ["node-1"]
  ```

  重新启动没有保存可以访问9200端口

  ![image-20220312221638472](https://gitee.com/tang-zhanpeng/blog-img/raw/master/img/image-20220312221638472.png)

# 二、集群配置

1、开放9200，9300，54328端口

```yaml
9200：  作为 Http 协议，主要用于外部通讯
9300：  数据传输端口：9300 用于集群之间交换数据
54328： 组播端口(UDP)
```

```shell
# 加入如下配置
#集群名称
cluster.name: cluster-es
#节点名称，每个节点的名称不能重复
node.name: node-1
#ip 地址，每个节点的地址不能重复
network.host: es1
#是不是有资格主节点
node.master: true
node.data: true
http.port: 9200
# head 插件需要这打开这两个配置
http.cors.allow-origin: "*"
http.cors.enabled: true
http.max_content_length: 200mb
#es7.x 之后新增的配置，初始化一个新的集群时需要此配置来选举 master
cluster.initial_master_nodes: ["node-1"]
#es7.x 之后新增的配置，节点发现
discovery.seed_hosts: ["es1:9300","es2:9300","es3:9300"]
gateway.recover_after_nodes: 2
network.tcp.keep_alive: true
network.tcp.no_delay: true
transport.tcp.compress: true
#集群内同时启动的数据任务个数，默认是 2 个
cluster.routing.allocation.cluster_concurrent_rebalance: 16
#添加或删除节点及负载均衡时并发恢复的线程个数，默认 4 个
cluster.routing.allocation.node_concurrent_recoveries: 16
#初始化数据恢复时，并发恢复线程的个数，默认 4 个
cluster.routing.allocation.node_initial_primaries_recoveries: 16
```

启动slave时报错：

```shell
[node-2] faile`在这里插入代码片`d to send join request to master [{node-1}{WbcP0pC_T32jWpYvu5is1A}{2_LCVHx1QEaBZYZ7XQEkMg}{10.10.11.200}{10.10.11.200:9300}], reason [RemoteTransportException[[node-1][10.10.11.200:9300][internal:discovery/zen/join]]; nested: IllegalArgumentException[can't add node {node-2}{WbcP0pC_T32jWpYvu5is1A}{p-HCgFLvSFaTynjKSeqXyA}{10.10.11.200}{10.10.11.200:9301}, found existing node {node-1}{WbcP0pC_T32jWpYvu5is1A}{2_LCVHx1QEaBZYZ7XQEkMg}{10.10.11.200}{10.10.11.200:9300} with the same id but is a different node instance]; ]

```

原因：准备机器的时候是克隆了主机的虚拟机，所以${ES_HOME}/data/下包含了主机的信息，清空时候，重新启动从机成功