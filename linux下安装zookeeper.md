[toc]

####  1、下载zookeeper

- 进入/opt/tool没有的话就新建一个文件夹

- https://archive.apache.org/dist/zookeeper/zookeeper-3.4.9/

- 本地下载好之后上传到/opt/tool目录下

  

#### 2、将文件解压到指定目录

```
tar -zxvf zookeeper-3.4.9.tar.gz 
mv zookeeper-3.4.9 /usr/local/tool
```

### 3、配置文件

```
#移到配置目录
cd /usr/local/tool/zookeeper-3.4.9/conf/
 
#复制配置文件
cp zoo_sample.cfg zoo.cfg
 
#修改及添加以下配置
tickTime=2000 
initLimit=10
syncLimit=5
dataDir=/opt/zookeeper/zoodata
dataLogDir=/opt/zookeeper/zoodatalog
clientPort=2181
server.0=127.0.0.1:2888:3888
 
#多节点 集群
#server.1=127.0.0.1:4888:5888
#server.2=127.0.0.1:5888:6888
 
#保存退出
:wq!
```

#### 4、创建节点的myid

```
#创建dataDir目录
mkdir -p /opt/zookeeper/zoodata
 
#移动到目录
cd /opt/zookeeper/zoodata
 
#把节点号写入myid文件(各个节点分别配置)
echo 0 > myid
 
#配置端口防火墙(各个节点分别配置)
firewall-cmd --zone=public --add-port=2181/tcp --permanent
firewall-cmd --reload
```

#### 5、启动

```

#移到执行目录
cd /opt/zookeeper/zookeeper-3.4.14/bin/
 
#启动服务
./zkServer.sh start

#重启
./zkServer.sh restart
 
#关闭
./zkServer.sh stop
 
#查看状态
./zkServer.sh staus
 
#启动的时候，查看后台信息
./zkServer.sh start-foreground &
```

#### 6、启动客户端

```
./zkCli.sh
```

#### 7、查看请求

![image-20220304210856577](https://gitee.com/tang-zhanpeng/blog-img/raw/master/img/image-20220304210856577.png)

