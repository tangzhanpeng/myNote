#### 1、下载jdk-8u321-linux-aarch64.tar.gz

#### 2、在linux中新建jdk目录

```
mkdir /opt/jdk
```

#### 3、通过sftp将下载好的jkd包上传到服务器root目录下

#### 4、将安装包移动到/opt/jdk

```
mv jdk-8u321-linux-aarch64.tar.gz /opt/jdk
```

#### 5、解压文件

```
tar -zxvf jdk-8u321-linux-aarch64.tar.gz
```

#### 6、新建安装目录

```
mkdir /usr/local/java
```

#### 7、将解压后的文件移动到安装目录下

```
mv jdk1.8.0_321 /usr/local/java
```

#### 8、配置环境变量

```
vim /etc/profile

export JAVA_HOME=/usr/local/java/jdk1.8.0_321
export PATH=$JAVA_HOME/bin:$PATH
```

#### 9、让配置文件生效

```
source /etc/profile
```

#### 10、查看java版本

```
java -version
```

![image-20220302214323856](https://gitee.com/tang-zhanpeng/blog-img/raw/master/img/image-20220302214323856.png)