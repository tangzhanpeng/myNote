[toc]

### 问题：node.js版本过高的问题

![img](https://gitee.com/tang-zhanpeng/blog-img/raw/master/img/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAc2FucWltYQ==,size_20,color_FFFFFF,t_70,g_se,x_16.png)

### 卸载node.js

```
sudo npm uninstall npm -g
sudo rm -rf /usr/local/lib/node /usr/local/lib/node_modules /var/db/receipts/org.nodejs.*
sudo rm -rf /usr/local/include/node /Users/$USER/.npm
sudo rm /usr/local/bin/node
sudo rm /usr/local/share/man/man1/node.1
sudo rm /usr/local/lib/dtrace/node.d

```

### 安装长期稳定版的的node

所有版本的网址：https://nodejs.org/zh-cn/download/releases/

![image-20220130194947675](https://gitee.com/tang-zhanpeng/blog-img/raw/master/img/image-20220130194947675.png)