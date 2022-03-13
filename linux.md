# 一、系统指令

```shell
shutdown -h now #立即关机
shutdown -h 1 "msg" #1分钟后关机
shutdown -r now #立即重启
halt #关机
reboot #重启
```

1.2常用命令记录：

```shell
cat /proc/sys/kernel/random/uuid  #生成uuid
```



# 二、用户相关

```shell
su - root                #切换身份
logout                   #登出当前用户
useradd *** -d /home/*** #新增用户 指定home目录
passwd pipi              #为某个用户设置密码
userdel -r pipi          #删除用户,保留home目录 -r删除home目录
who am i                 #查询登陆信息
########### 用户组（角色权限）###########
groupadd **                 #新增组
groupdel **                 #删除组
useradd -g t_group xiaolan  #将用户添加到某个组
usermod -g mojiao xiaolan   #修改用户所在组
id **                       #查看用户所在组
############### 用户文件 #####################
/etc/passwd  #用户配置文件
/etc/shadow  #口令配置文件
/etc/group   #组配置文件

```

# 三、目录管理

## 1、目录类型

- pwd查看绝对路径 /home/tom   
- 相对路径 当前如果在/home/tom目录下，前往home目录指令：  cd ../../home

## 2、操作目录

```shell
mkdir /home/dog #创建一级目录
mkdir -p /home/cat/fish #创建多级目录，
rmdir /home/dog #删除空目录
rm -rf /home/cat #删除非空目录 递归删除，谨慎使用
touch c.txt #创建空文件
cp c.txt /bbb #复制文件到指定目录
cp -r ddd asd #递归复制文件夹到指定目录
mv ddd ccc #重命名，同目录下
mv ddd /home/ddd
rm -rf * #清空当前目录下所有文件
```

## 3、查看指令

```shell
cat 、more、 less
echo $PATH #输出内容到控制台
echo 'a' > *** #输入内容到指定文件===覆盖
echo 'a' >> *** #追加内容
head -n 10 #查看文件前n行文件，默认10行
tail -n 10 #查看后n行文件
tail -f 10 #监控文件内容，查看日志
ls -lh  #大小可以人看的

```

## 4、重定向

```shell
>    #覆盖
>>   #追加
cat a.txt >> b.txt
echo 'a' >> a.txt
```

## 5、其它指令

```shell
ln -s /root /home/myroot       #符号连接，类似快捷方式
rm /home/myroot                #删除软连接
history         history 10     #历史指令
!10 #执行指令编号的指令
```

# 四、时间日期类

```shell
date  #显示当前时间
date "+%Y-%m-%d %H:%M:%S" #显示指定格式的时间
date -s "2022-03-09 17:25:00"   #设置指定的日期
cal #查看当前月日历
cal 2022 #查看年厉
```

# 五、查找指令

```shell
#find指令：
find /home -name hello.java #根据名字
find /opt -user nobody  #根据用户查找
find / -size +200M  #根据大小查找
#locate 更具数据库进行快速检索
updatedb
locate hello.java
#which 查看某个指令在那个文件目录下
which ls
#grep指令，过滤指令，一般配合管道符号使用｜
cat hello.java | grep "hello"  #查看hello.java中的内容，包含有hello
grep -n "hello" hello.java     #和上面的想过一样

```

# 六、压缩和解压

```shell
#zip指令
gzip hello.java                   #压缩文件
zip -r myhome.zip /home/tommy/   #将指定文件夹下所有压缩到成指定文件
gunzip hello.java.gz             #解压文件
unzip myhome.zip -d /home/tommy/test  #解压

#tar指令 可以压缩也可以解压
tar [选项] xxx.tar.gz 打包的内容
-c 产生.tar打包文件
-v 显示详细信息
-f 指定压缩后的文件名
-z 打包同时压缩
-x 解包.tar文件
tar -zcvf pc.tar.gz pig.txt cat.txt  #将多个文件打包
tar -zcvf myhome.tar.gz /home        #打包目录下所有文件
tar -zxvf pc.tar.gz                  #解压
tar -zxvf pc.tar.gz -C /opt/tem1     #解压到指定目录
```

# 七、权限、组

## 1、组相关

- linux中每个用户必须属于一个组

- linux中文件都有一个所有者，所有组、其他组

  > 用户创建的文件，该文件默认属于该用户的所在组
  >
  > 但是文件是可以修改所在组的

```shell
ls -ahl                 #显示文件的所有者
chown tommy pc.tar.gz   #修改文件的所有者
groupadd monster        #创建组
useradd -g monster fox  #创建用户指定一个组
id fox                  #查看用户信息
passwd fox              #设置密码
chgrp fruit orange.txt  #修改文件所在组
usermod -g 新组名 用户名  #修改用户所在组
usermod -g 新目录 用户名  #修改用户登陆的初始目录，说明：用户需要有进入新目录的权限
```

## 2、权限相关

- drwx------.  5 fox     monster 4096 3月   9 18:34 fox

  > 1）第0位确定文件类型（d,-,l,c,b）
  >
  > l是链接，相当于跨界方式
  >
  > -指普通文件
  >
  > d表示目录
  >
  > c是字符设备文件，鼠标、键盘
  >
  > b是块设备，如硬盘
  >
  > 2）第1-3位确定所有者（该文件的所有者）拥有该文件的权限 ---user
  >
  > 3）第4-6位确定所属组拥有该文件的权限 ---group
  >
  > 4）第7-9位确定其他用户拥有该文件的权限 ---other

- rwx作用到文件

  > [r]代表可读（read）:可以读取，查看
  >
  > [w]代表可写(write):可以i修改，但是不代表可以删除文件，删除i个文件的前提条件是对该文件所在的目录有写权限，才能删除改文件
  >
  > [x]代表可以执行（execute）： 可以呗执行

- rwx作用到目录

  >1、【r】代表可以读：可以读取，ls查看目录内容
  >
  >2、【w】代表可写：可以修改，对目录内创建+删除+重命名目录
  >
  >3、【x】代表可执行：可以进入该目录
  
- selinux问题

  > vim /etc/sysconfig/eselinux
  >
  > 

```shell
chmod指令
chmod u=rwx,g=rx,ox 文件/目录名
chmod o+w 文件/目录名
chmod a-x 文件/目录名
1)给abc文件的所有者读写执行的权限，给所在组读执行权限，给其它组读执行权限。
chmod u=rwx,g=rx,o=rx abc
2)给abc文件的所有者除去执行的权限，增加组写的权限
chmod u-x,g+w abc
3) 给 abc 文件的所有用户添加读的权限
chmod a+r abc
===通过数字的方式修改权限===
chmod 755 a.txt
#修改文件的所有者
chown tom /home/a.txt
chown -R tom /kkk #修改目录下所有的文件的所有者 
chown -R tom:tom /kkk/
#修改文件/目录所在组
chgrp tom 文件/目录
chgrp -R tom 文件/目录
```

# 八、定时调度coud

```
crontab -l -e -r
at atq qtrm
```

# 九、磁盘分区

```shell
lsblk #查看分区详情
lsblk -f 更详细的信息
df -h #查看磁盘使用情况
-s 指定目录大小
ls -l /etc | grep "^-" | wc -l #统计文件数量
ls -l /etc | grep "^d" | wc -l #统计目录个数
ls -lR /etc | grep "^-" | wc -l #递归统计文件数量、递归
ls -lR /etc | grep "^d" | wc -l #递归统计目录数量、递归
tree /etc #树状
```

# 十、网络配置

## 1、设置网络

>Mac 关于设置vmware fusion 网络设置参考： https://blog.csdn.net/qq_37960324/article/details/82622553

```shell
ifconfig 、id addr #查看本机ip
cd /etc/sysconfig/network-scripts/ #查看网卡类型 ens33 ens160等
vim /etc/sysconfig/network-scripts/ifcfg-ens160

route -n  #查看默认网关

TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=static  #设置为指定模式
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
###添加以下内容
IPADDR=10.0.0.100  #ip地址
GATEWAY=10.0.0.2  #网关
NETMASK=255.255.255.0
DNS1=8.8.8.8

####重启网络
service network restart
ping 10.0.0.1 #测试本机
ping 10.20.0.20 #测试内网
ping www.baidu.com #测试外网 ok.....
```

## 2、设置主机名

```shell
vim /etc/hostname #修改主机名
reboot #修改后重启
hostname  #查看主机名

#修改mac的hosts文件
sudo vim /etc/hosts
127.0.0.1       localhost
255.255.255.255 broadcasthost
::1             localhost
127.0.0.1 eureka7001.com
127.0.0.1 eureka7002.com
10.0.0.100 study #添加映射关系
####可以使用主机名进行连接了哈哈哈哈
ping study
```

# 十一、进程管理

## 1、常用

```shell
 #ps指令
 ps -a                 #显示当前终端的所有进程信息
 ps -u                 #以用户的格式显示进程信息
 ps -x                 #显示后台进程运行的参数
 ps -aux | grep ***    #过滤
 ps -ef | grep **.     #以全格式显示所有线程
 
 #杀死进程
kill [指令] [进程id]
killall             #杀死所有线程包括子线程
-9                  #强制杀死 
ps -ef | grep sshd  #查看远程登录服务，kill掉则无法远程登录
systemctl start sshd.service #启动远程登录服务
killall name        #根据进程名称杀死所有
pe -ef | grep bash  #查看终端服务
kill -9 ***.        #需要强制杀死

#树状显示
pstree -p
pstree -u
```

## 2、services服务管理

```shell
service 服务名 【start|stop|restart|reload|status】
#cenos7.0后很多服务不再使用service,而是systemctl
ls -l /etc/init.d/ #查看还可以使用service的服务
setup #查看系统所有服务，可以设置开机启动的服务
```

- 运行级别

  > Linux 系统有 7 种运行级别(runlevel)：
  > ：常用的是级别3和5
  > 运行级别0：系统停机状态，系统默认运行级别不能设为0，否则不能正常启动
  > 运行级别1：单用户工作状态，root权限，用于系统维护，禁止远程登陆
  > 运行级别2：多用户状态(没有NFS)，不支持网络
  > 运行级别3：完全的多用户状态(有 NFS)，无界面，登陆后进入控制台命令行模式
  > 运行级别4：系统未使用，保留
  > 运行级别5：X11控制台，登陆后进入图形GUI模式运行级别6：系统正常关闭并重启，默认运行级别不能设为6，否则不能正常启动 

  ```shell
  int[0123456]
  systemctl get-default #查看运行级别
  systemctl set-default multi-user.target #修改默认级别
  systemctl set-default graphical.target #修改默认级别
  ```

- chkconfig指令

  ```shell
  #chkconfig命令可以给服务的各个运行级别设置 启动/关闭
  chkconfig --list [| grep ***] #查看服务运行级别
  chkconfig 服务名 --list
  chkconfig --level 5 服务名1 on/off
  #说明：chkconfig设置之后需要reboot才能生效
  ```

- systemctl指令

  > systemctl 【start|stop|restart|reload|status】服务名
  > systemctl指令管理的服务子在/usr/lib/systemd/system 查看

  ```shell
  systemctl list-unit-files | grep firewalld   #查看某个服务的指令
  systemctl enable 服务名     #设置服务开机启动 cenos7后默认运行级别为3，5都生效
  systemctl disable 服务名    #关闭服务开机启动
  systemctl is-enable 服务名  #查询某个服务是否是自启动的
  systemctl stop firewalld   #关闭防火墙
  systemctl start firewalld  #开启防火墙3、防火墙
  ```

## 3、防火墙

```shell
netstat -anp | more   #查看网络监听情况
telnet 10.0.0.100 111 #访问某个端口
firewall-cmd --permanent --add-port=端口号/协议     #打开端口
firewall-cmd --permanent --remove-port=端口号/协议  #关闭端口
firewall-cmd --reload                             #重新载入，才能生效
firewall-cmd --query-port=端口号/协议               #查看端口是否开放
firewall-cmd --list-all
```

# 十二、动态监控

## 1、监控进程

> top与 ps 命令很相似。它们都用来显示正在执行进程
> Top与ps最大的不同之处，在于top在执行一段时间可以更新正在运行的的进程。

```
top
P    以cup使用率排序、某人此项
M    以内存使用率排序
N    以PID排序
q    退出
u    输入用户名查看用户的进程
k    输入信号量之后杀掉进程
top -d 10  	每隔10s自动刷新，某人为3秒
```

## 2、网络情况

```shell
netstat -anp | more
netstat -anp | grep sshd
```

# 十三、rpm包的管理

```shell
rpm -qa | grep firefox    #查看是否安装某个rpm包
rpm -q firefox
rpm -qi firefox          #详细信息
#软件包名 :查询软件包中的文件
rpm -ql firefox
rpm -qf 文件全路径名 查询文件所属的软件包
rpm -qf /etc/passwd
rpm -qf /root/install.log
######卸载
rpm -e 包名
rpm -e --nodeps foo #强制卸载忽略依赖
######安装
rpm -ivh 包全路径
参数说明：
i=install 安装
v=verbose 提示
h=hash    进度条
```

# 十四、yun管理

```shell
yum list | grep xx软件  #查看yum是否有安装包
yum install xxx #下载安装
```

# 十五、安装jdk

[jdk安装](linux下安装jdk.md)
