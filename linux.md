#### 1、关机

```
shutdown -h now
```

#### 2、修改ip配置

```
vim /etc/sysconfig/network-scripts/ifcfg-ens160

TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=static
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
NAME=ens160
UUID=13980af8-5521-4634-9daa-c75b03bb7c3c
DEVICE=ens160
ONBOOT=yes
IPADDR=10.20.0.38
GATEWAY=10.20.0.1
NETMASK=255.255.255.0
```

#### 3、重启网络

```
service network restart
```

#### 4、查看ip信息

```
ip addr
```

#### 5、修改主机名称

```
vim /etc/hostname
```

#### 6、查看主机名

```
hostname
```

#### 7、重启服务器

```
reboot
```

#### 8、修改host文件

```
sudo vim /etc/hosts
127.0.0.1       localhost
255.255.255.255 broadcasthost
::1             localhost
127.0.0.1 eureka7001.com
127.0.0.1 eureka7002.com
```

#### 9、关闭防火墙

```
systemctl stop firewalld
systemctl status firewalld
```

#### 10、创建、删除文件夹

```
mkdir -p zookeeper
```

