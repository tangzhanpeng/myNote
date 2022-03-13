# nginx部署高可用相关

## keepalived配置文件：

```shell
! Configuration File for keepalived
global_defs {
     notification_email {
     root@localhost #健康检查报告通知邮箱
}
notification_email_from keepalived@localhost #发送邮件的地址
     smtp_server 127.0.0.1 #邮件服务器
     smtp_connect_timeout 30
     router_id LVS_DEVEL
     script_user root
     enable_script_security
}

vrrp_script chk_nginx {
     script "/data/sh/check_nginx.sh" ##检查本地nginx是否存活脚本需要自己写，后面会有该脚本内容
     interval 2
     weight 2
}

#VIP1
vrrp_instance VI_1 {
     state BACKUP
     interface ens160
     virtual_router_id 51
     priority 90
     advert_int 1 #同步检测频率
     authentication {
          auth_type PASS
          auth_pass 1111
     }
     virtual_ipaddress {
          10.0.0.250 ##VIP
     }
     track_script {
          chk_nginx
     }
}
```

检测nginx存活脚本

```shell
#!/bin/bash
A=$(ps -C nginx --no-heading |wc -l)
echo "$A"
if [ $A -eq 0 ];then
        nginx
        sleep 2
        A=$(ps -C nginx --no-heading |wc -l)
        if [ $A -eq 0 ];then
                pkill keepalived
        fi
fi
```

主机宕机后ip慢漂移问题，通过如下脚本解决(没卵用，一堆垃圾博客)

```shell
#!/bin/bash
VIP=10.0.0.250
GATAWAY=10.0.0.2
/sbin/arping -I ens160 -c 5 -s $VIP $GATEWAY &>/dev/null


#keepalived配置文件添加内容
vrrp_sync_group VG1 {
    group {
          VI_1
    }
notify_master "/data/sh/arp.sh"  #当切换到MASTER时,运行的脚本
}
```

cenos7允许防火墙vrrp多播（解决争夺虚拟ip问题）

```shell
firewall-cmd --direct --permanent --add-rule ipv4 filter INPUT 0 --destination 224.0.0.18 --protocol vrrp -j ACCEPT
firewall-cmd --direct --permanent --add-rule ipv4 filter OUTPUT 0 --destination 224.0.0.18 --protocol vrrp -j ACCEPT
firewall-cmd --reload
```