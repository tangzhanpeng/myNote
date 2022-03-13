- ./xxx.sh 执行需要操作权限 chmod u+x xxx.sh

- sh xxx.sh 不需要操作权限

- 系统变量

  > $HOME,$pwd,$SHELL,$USER等

- shell变量的定义

  >基本语法
  >
  >1、定义变量：变量名=值
  >
  >2、撤销变量：unset 变量
  >
  >3、声明静态变量：readonly变量，注意：不能unset

```shell
A=100
#输出变量需要$
echo A=$A
echo "A=$A"
unset A
echo "A=$A"

readonly B=2
echo "B=$B"
```

实用的shell案例：

### 1、备份数据库脚本

```shell
#!/bin/bash
#备份目录
BACKUP=/data/backup/db
#当前时间
DATETIME=$(date +%Y-%m-%d_%H%M%S)
echo $DATETIME
#数据库用户名
DB_USER=root
#数据库密码
DB_PW=tang0011
#备份的数据库名
DATEBASE=test01

#创建备份目录，如果不存在，就创建
[ ! -d "${BACKUP}/${DATETIME}" ] && mkdir -p "${BACKUP}/${DATETIME}"

#备份数据库
mysqldump -u${DB_USER} -p${DB_PW} --host=${HOST} -q -R --databases ${DATEBASE} | gzip > ${BACKUP}/${DATETIME}/$DATETIME.sql.gz

#将文件处理成tar.gz
cd ${BACKUP}
tar -zcvf $DATETIME.tar.gz
#删除备份的目录
rm -rf ${DATETIME}

#删除10天前的备份文件
find ${BACKUP} -atime +10 -name "*.tar.gz" -exec rm -rf {} \;
echo "备份数据库${DATEBASE}成功"
```

