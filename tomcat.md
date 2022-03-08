[toc]

### 一、下载tomcat

1、首先到官网下载Tomcat：[https://tomcat.apache.org/download-90.cgi]

![image-20220130153231195](https://gitee.com/tang-zhanpeng/blog-img/raw/master/img/image-20220130153231195.png)



2、解压tomcat文件,最好把它文件名重命名为“tomcat9”，方便以后查找，最后把它放入/event/tomcat9

![image-20220130153626247](https://gitee.com/tang-zhanpeng/blog-img/raw/master/img/image-20220130153626247.png)

### 二、用终端（Terminal）直接打开Tomcat了

1、进入Tomcat的bin目录下：终端输入cd event/tomcat9/bin ，输完回车

```
cd event/tomcat9/bin
```

2、授权bin目录下的所有操作：终端输入sudo chmod 755 *.sh，输完回车

```
sudo chmod 755 *.sh
```

3、这时要输入密码，输完回车

4、这时候就可以开启Tomcat了，终端输入sudo sh ./startup.sh，输完回车

```
sudo sh ./startup.sh
```

![img](https://gitee.com/tang-zhanpeng/blog-img/raw/master/img/webp)

### 三、到浏览器输入网址localhost，若出现了下面的画面就证明成功了

![img](https://gitee.com/tang-zhanpeng/blog-img/raw/master/img/webp-20220130154100227)

### 四、关闭Tomcat，用终端输入sh ./shutdown.sh，回车即可关闭

*说明：*

*sudo为系统超级管理员权限.*

*chmod 改变一个或多个文件的存取模式*

*755代表用户对该文件拥有读、写、执行的权限，同组的其他人员拥有执行和读的权限，没有写的权限，其它用户的权限和同组人员一样.*

*777代表，user,group ,others ,都有读写和可执行权限.*

*chmod -R 777 folername,获取文件夹权限.*