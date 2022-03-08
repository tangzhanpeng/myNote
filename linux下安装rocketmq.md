[toc]



#### 1、下载rocketmq

```
https://archive.apache.org/dist/rocketmq/4.9.2/rocketmq-all-4.9.2-bin-release.zip
```

#### 2、linux下新建目录

```
mkdir /opt/tool
```

#### 3、通过sftp将下载好的包放到上面目录中

![image-20220302214812115](https://gitee.com/tang-zhanpeng/blog-img/raw/master/img/image-20220302214812115.png)

#### 4、新建安装目录

```
mkdir /usr/local/tool
```

#### 5、解压文件到指定目录

```
unzip  rocketmq-all-4.9.2-bin-release.zip -d /usr/local/tool
```

#### 6、修改bin目录下的配置文件

![image-20220302215756581](https://gitee.com/tang-zhanpeng/blog-img/raw/master/img/image-20220302215756581.png)

- 修改runserver.sh

  ```
  vim runserver.sh
  ```

  ![image-20220302220358630](https://gitee.com/tang-zhanpeng/blog-img/raw/master/img/image-20220302220358630.png)

- 修改runbroker.sh

  ```
  vim runbroker.sh
  ```

  ![image-20220302220636286](https://gitee.com/tang-zhanpeng/blog-img/raw/master/img/image-20220302220636286.png)

#### 7、启动NameServer

```
nohup sh bin/mqnamesrv & 
tail -f ~/logs/rocketmqlogs/namesrv.log
```

#### 8、启动broker

```
nohup sh bin/mqbroker -n localhost:9876 &  
#或者如下修改conf/broker.conf
namesrvAddr=192.168.1.5:9876
brokerIP1=192.168.1.5
#然后通过指定配置文件的方式启动broker
nohup sh bin/mqbroker -c conf/broker.conf &

tail -f ~/logs/rocketmqlogs/broker.log
```

#### 9、因为4.9.2版本的mq采用的默认垃圾收集器使用的g1，如果是在jdk8环境下运行需要增加jvm参数

```
 -XX:+UnlockExperimentalVMOptions
```

#### 10、关闭服务器

```
sh bin/mqshutdown broker
sh bin/mqshutdown namesrv
```

#### 11、测试

```
export NAMESRV_ADDR=10.20.0.35:9876
sh bin/tools.sh org.apache.rocketmq.example.quickstart.Producer
sh bin/tools.sh org.apache.rocketmq.example.quickstart.Consumer
```

#### 12、记得关闭防火墙如果是测试的话，生成环境到时候再看吧

```
systemctl stop firewalld
systemctl status firewalld
```

#### 13、集群配置

配置方式/2m-2s-async

##### 1、配置第一台主机

- **broker-b-s.properties**

  ```
  brokerClusterName=DefaultCluster
  brokerName=broker-b
  brokerId=1
  deleteWhen=04
  fileReservedTime=48
  brokerRole=SLAVE
  flushDiskType=ASYNC_FLUSH
  namesrvAddr=10.20.0.35:9876;10.20.0.38:9876
  listenPort=11911 
  storePathRootDir=~/store-s 
  storePathCommitLog=~/store-s/commitlog 
  storePathConsumeQueue=~/store-s/consumequeue 
  storePathIndex=~/store-s/index 
  storeCheckpoint=~/store-s/checkpoint 
  abortFile=~/store-s/abort
  ```

  

- **broker-a.properties**

  ```
  brokerClusterName=DefaultCluster
  brokerName=broker-a
  brokerId=0
  deleteWhen=04
  fileReservedTime=48
  brokerRole=SYNC_MASTER
  flushDiskType=ASYNC_FLUSH
  namesrvAddr=10.20.0.35:9876;10.20.0.38:9876
  ```

##### 2、配置第二台主机

- **broker-b.properties**

  ```
  brokerClusterName=DefaultCluster
  brokerName=broker-a
  brokerId=0
  deleteWhen=04
  fileReservedTime=48
  brokerRole=SYNC_MASTER
  flushDiskType=ASYNC_FLUSH
  namesrvAddr=10.20.0.35:9876;10.20.0.38:9876
  
  ```

- **broker-b-s.properties**

  ```
  brokerClusterName=DefaultCluster
  brokerName=broker-a
  brokerId=1
  deleteWhen=04
  fileReservedTime=48
  brokerRole=SLAVE
  flushDiskType=ASYNC_FLUSH
  namesrvAddr=10.20.0.35:9876;10.20.0.38:9876
  listenPort=11911 
  storePathRootDir=~/store-s 
  storePathCommitLog=~/store-s/commitlog 
  storePathConsumeQueue=~/store-s/consumequeue 
  storePathIndex=~/store-s/index 
  storeCheckpoint=~/store-s/checkpoint 
  abortFile=~/store-s/abort
  ```

##### 3、启动集群sevices

```
nohup sh bin/mqnamesrv &
tail -f ~/logs/rocketmqlogs/namesrv.log
```

##### 4、**启动两个Master**

```
nohup sh bin/mqbroker -c conf/2m-2s-async/broker-a.properties &
tail -f ~/logs/rocketmqlogs/broker.log

nohup sh bin/mqbroker -c conf/2m-2s-async/broker-b.properties &
tail -f ~/logs/rocketmqlogs/broker.log 
```

##### 5、**启动两个Slave**

```
nohup sh bin/mqbroker -c conf/2m-2s-async/broker-b-s.properties &
tail -f ~/logs/rocketmqlogs/broker.log

nohup sh bin/mqbroker -c conf/2m-2s-async/broker-a-s.properties & 
tail -100f ~/logs/rocketmqlogs/broker.log
```

