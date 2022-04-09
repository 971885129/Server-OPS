# HPC-cluster-build
## 测试搭建
### KVM虚拟机安装
#### 环境准备

    cat /etc/redhat-release
    ifconfig
    getenforce #检查SElinux是否启动
    关闭SElinux
    setenforce 0 #临时关闭
    #安装相关软件包
    yum install qemu-kvm libvirt virt-install virt-manager bridge-utils
    service libvirtd start
    #创建网桥
    

    #创建安装磁盘
    mkdir -p /media/sdb2/linux/kvm1
    qemu-img create -f qcow2 /var/linux/images/centos74.img 20G
        Formatting '/media/sdb2/linux/kvm1/centos74.img', fmt=qcow2 size=21474836480 encryption=off cluster_size=65536 
    #安装虚拟机
    virt-install \
        --name kvm1 \
        --ram 1024 \
        --vcpus 1 \
        --disk path=/media/sdb2/linux/kvm1/centos74.img,size=10 \
        --os-type linux \
        --os-variant rhel7 \
        --graphics none \
        --console pty,target_type=serial \
        --location=/var/CentOS-7-x86_64-DVD-1810.iso \
        --extra-args 'console=ttyS0,115200n8 serial'
    #报错
    qemu-kvm: could not open disk image ' ': Permission denied
    #解决
    Changing /etc/libvirt/qemu.conf to make things work
        # The user for QEMU processes run by the system instance. It can be
        # specified as a user name or as a user id. The qemu driver will try to
        # parse this value first as a name and then, if the name doesn't exist,
        # as a user id.
        #
        # Since a sequence of digits is a valid user name, a leading plus sign
        # can be used to ensure that a user id will not be interpreted as a user
        # name.
        #
        # Some examples of valid values are:
        #
        #       user = "qemu"   # A user named "qemu"
        #       user = "+0"     # Super user (uid=0)
        #       user = "100"    # A user named "100" or a user with uid=100
        #
        user = "root"

        # The group for QEMU processes run by the system instance. It can be
        # specified in a similar way to user.
        group = "root"

        # Whether libvirt should dynamically change file ownership  
        # to match the configured user/group above. Defaults to 1.
        # Set to 0 to disable file ownership changes.
        #dynamic_ownership = 1
    service libvirtd restart
 
### 重新安装在R730（CentOS 7）

* 环境准备

        getenforce    #检查SELinux是否关闭
        找到/etc/selinux/config 文件把文件中的SELINUX=enforcing   改为SELINUXdisabled
        重启
        
        systemctl status firewalld.service   #检查
        systemctl stop firewalld.service #关闭防火墙
        systemctl disable firewalld.service #防止开机自动重启防火墙
        service NetworkManager stop 
* 安装相关软件包

        yum install qemu-kvm libvirt virt-install virt-manager bridge-utils
        说明：qemu-kvm    ----模拟计算机的工具，为KVM虚拟机提供IO设备

           libvirt    ----管理虚拟机

           virt-install    ----命令行的虚拟机创建安装工具

           bridge-utils   ----网桥工具
* 启动

        systemctl start libvirtd
* 创建网桥

        #编辑ifcfg-em2(当前使用网卡配置文件），修改以下部分
        BOOTPROTO=none
        BRIDGE=br0
        
        #新建ifcfg-br0（ip与现用网卡一致）
        DEVICE=br0
        ONBOOT=yes
        TYPE=Bridge
        BOOTPROTO=static
        IPADDR=172.16.0.99
        NETMASK=255.255.255.0
        GATEWAY=172.16.0.1
        DEFROUTE=yes

* 重启

        reboot
        #本次未执行重启，仅重启网络
        service network restart
* 创建安装盘

        mkdir -p /var/linux/img
        cd /var/linux/img
        qemu-img create -f qcow2 /var/linux/images/centos74.img 20G
* 安装虚拟机

        virt-install \
            --name kvm1 \
            --ram 1024 \
            --vcpus 1 \
            --disk path=/var/linux/img/centos74.img,size=10 \
            --os-type linux \
            --os-variant rhel7 \
            --graphics none \
            --console pty,target_type=serial \
            --location=/var/linux/CentOS-7-x86_64-Minimal-1810.iso \
            --extra-args 'console=ttyS0,115200n8 serial'
### 存在问题

* 无法连接网络

        #解决
        设置/etc/resolv.conf DNS
            nameserver 202.114.64.1
        重启网络
            service network restart


* KVM相关命令


