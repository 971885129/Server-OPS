# SSH服务

### 修改端口

修改/etc/ssh/sshd_config，增加Port 2124（取消Port 22的注释，防止新增加的端口不可用）

    vim /etc/ssh/sshd_config
    #Port 22
    Port 2124
    #AddressFamily any
    #ListenAddress 0.0.0.0
    #ListenAddress ::

重启SSH服务

    service sshd restart

修改防火墙

    vim /etc/sysconfig/iptables
    增加-A INPUT -m state --state NEW -m tcp -p tcp --dport 2124 -j ACCEPT
    注：该行命令必须放在如下两行之前
      -A INPUT -j REJECT --reject-with icmp-host-prohibited
      -A FORWARD -j REJECT --reject-with icmp-host-prohibited

重启防火墙

    service iptables restart
# 服务器运维

设置自动挂载

    修改/etc/fstab
    /dev/sdi /media/sda auto nosuid,nodev,nofail,x-gvfs-show 0 0

### 用户设置

增加用户组

    groupadd manage

增加用户

    useradd:
          -g:设置用户所属用户组
          -d:设置用户主目录（一般设置在/home目录下）
          -m:若用户主目录不存在则创建（后不加参数）
          -G:指定用户附加组
    示例：
         useradd -g manage -d /home/username -m username 

设置密码

    /bin/passwd username
    初始密码:password_HTP_<date>
    /usr/lib64/yp/ypinit
    /etc/passwd 存放账户信息
    /etc/shadow  存放用户密码
    /etc/group 用户组信息

修改用户所属组

    usermod -g manage wxm #wxm用户组修改为manage

删除用户

    #仅删除用户
    userdel username
    #删除用户及home目录下文件
    userdel -r username

删除用户组

    groupdel wxm    

修改UID

    usermod -u 1015 syf #syf的用户ID修改为1015

修改GID

### 服务器IP设置

小服务器命令行设置IP

    #查看网络连接详情
    ifconfig
    #进入设置对应网卡目录
    cd /etc/sysconfig/network-scripts/
    #编辑指定网卡配置文件
    vim ifcfg-eth1
    #修改
    IPADDR=210.42.112.130
    GATEWAY=201.42.112.254
    DNS1=202.114.96.1
    NETMASK=255.255.255.0

### 硬盘卸载和缓存释放

戴尔存储阵列硬盘在短期大量读写会产生较多缓存需清理，长时间会自动清理

    # 检测硬盘是否被占用
    fuser -cu /media/sdb
    # 删除占用程序
    fuser -ck /media/sdb
    # 卸载
    unmount /media/sdb
    # fdisk -l 

释放缓存
    
    # 释放缓存期间禁止登录，创建nologin目录，并移到etc下
    /etc/nologin 该目录下内容会在用户登录时提示
    # 释放所有缓存,释放之前尽量解挂所有硬盘
    nohup fstrim -a -v >>20200620.o 2>20200620.e &

    rm /etc/nologin

### 特殊权限设置

SUID，表现在文件所有者的执行权限位上显示为s。SUID权限只对二进制文件有效，s权限的具体含义是，当某个文件的拥有者执行权限位是s的话，其他用户执行这个二进制时，在执行期间，用户获得文件拥有者的权限。

    增加SUID权限可以通过chmod 4xxx <文件名> 来进行

### 问题

umount 显示被占用

    #检查占用用户及进程
    fuser -mv /media/sda/

用户自行设置密码，报错

    Authentication token manipulation error

    # 解决
    # 可能问题1.密码文件权限问题
    查看/etc/passwd /etc/shadow文件的权限中是否有i
    lsattr /etc/passwd /etc/shadow
    若有则修改
    chattr -i /etc/shadow
    chattr -i /etc/passwd

    # 可能问题2./usr/bin/passwd文件权限问题
    用户自行修改权限需通过/usr/bin/passwd文件设置，该文件权限出问题会导致报错
    修改权限chmod 4755 /usr/bin/passwd






