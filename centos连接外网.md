###### VirtualBox虚拟机配置
> 首先虚拟机网络配置NAT或者桥接网卡。NAT方式就是路由转换，本地生成一个路由地址供centos使用。桥接网卡方式就是和本地一样IP段，连接外网。
###### centos配置
```bash
[**@** /]$ cat /etc/sysconfig/network-scripts/ifcfg-enp0s3 #修改配置文件，开机自动加载网络
ONBOOT=yes　　#系统启动时是否自动加载
[**@** /]$ service network restart  #重启网卡
[**@** /]$ ifconfig  #查看ip地址
[**@** /]$ ping www.baidu.com  #查看是否连接外网
```

