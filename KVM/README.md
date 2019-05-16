# 安装KVM虚拟机

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


### kvm下挂载新虚拟硬盘

* 创建虚拟硬盘

        qemu-img create -f qcow2 new.img 40G
* 修改虚拟机配置文件

        cd /etc/libvirt/qemu/
        virsh edit kvm1 
            <disk type='file' device='disk'>
              <driver name='qemu' type='qcow2'/>
              <source file='/var/linux/img/centos74.img'/>
              <target dev='vda' bus='virtio'/>
              <address type='pci' domain='0x0000' bus='0x00' slot='0x06' function='0x0'/>
            </disk>
        在上面内容后添加：
            <disk type='file' device='disk'>
              <driver name='qemu' type='qcow2'/>
              <source file='/var/linux/img/new.img'/>
              <target dev='vdb' bus='virtio'/>
            </disk>
* 启动并登陆虚拟机

        virsh start kvm1
        virsh console kvm1
* 查看是否已经添加新的硬盘
fdisk -l

        Disk /dev/vdb: 42.9 GB, 42949672960 bytes, 83886080 sectors
        Units = sectors of 1 * 512 = 512 bytes
        Sector size (logical/physical): 512 bytes / 512 bytes
        I/O size (minimum/optimal): 512 bytes / 512 bytes
* 格式化新的硬盘为ext4格式

        mkfs.ext4 /dev/vdb
* 挂载

        mount /dev/vdb /media/
* 备注

        lsblk -f 可查看所有硬盘、分区、文件系统格式等




### 存在问题

* 无法连接网络

        #解决
        设置/etc/resolv.conf DNS
            nameserver 202.114.64.1
        重启网络
            service network restart


* KVM相关命令
