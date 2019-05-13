# 服务器网络设置
* 在CentOS系统上，目前有NetworkManger和network两种网络管理工具，如果两种都配置会起冲突，一般可将NetworkManager禁用。

      service NetworkManager stop
* 通过network配置网络由两部分组成
  1.设置/etc/sysconfig/network-scripts/ifcfg-eth1
  
      DEVICE=eth1     #设备名称
      TYPE=Ethernet   #网卡协议类型
      UUID=beb3242d-101c-455c-8ea8-bb98cf40f3e9   #
      **ONBOOT=yes  #是否激活该网卡**
      **NM_CONTROLLED=no #是否由NetworkManager控制该网络接口**
      **BOOTPROTO=static #获取地址协议【static静态协议】 【bootp协议】 【dhcp动态协议】**
      DEFROUTE=yes
      IPV4_FAILURE_FATAL=yes
      IPV6INIT=no
      NAME="System eth1"
      **IPADDR=172.16.0.97 #IP地址**
      PREFIX=24
      **NETMASK=255.255.255.0 #子网掩码**
      **GATEWAY=172.16.0.1 #网关**
      #DNS1=202.114.64.1 
      HWADDR=D0:50:99:52:13:6E #指定MAC地址，不能和MACADDR一起使用
      LAST_CONNECT=1457680358

  2.设置DNS /etc/resolv.conf

      nameserver 202.114.64.1
* 重启网卡

      service network restart

* 关闭NetworkManger后，应将不用的网卡设置为ONBOOT=no,否则重启网卡会导致冲突
*



