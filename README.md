# Server-operation-and-maintenance

* 自动挂载

      修改/etc/fstab
      /dev/sdi /media/sda auto nosuid,nodev,nofail,x-gvfs-show 0 0
* umount 显示被占用

      #检查占用用户及进程
      fuser -mv /media/sda/

## 用户设置
* 增加用户组

      groupadd manage
* 增加用户

      useradd:
            -g:设置用户所属用户组
            -d:设置用户主目录（一般设置在/home目录下）
            -m:若用户主目录不存在则创建（后不加参数）
            -G:指定用户附加组
      示例：
            useradd -g manage -d /home/username -m username 
* 设置密码

      /bin/passwd username
      初始密码:password_HTP_123
      /usr/lib64/yp/ypinit
      
   * 用户自行设置密码，报错
   
            Authentication token manipulation error
   * 可能问题1.密码文件权限问题
   
            查看/etc/passwd /etc/shadow文件的权限中是否有i
            lsattr /etc/passwd /etc/shadow
            若有则修改
            chattr -i /etc/shadow
            chattr -i /etc/passwd
   * 可能问题2./usr/bin/passwd文件权限问题
   
            用户自行修改权限需通过/usr/bin/passwd文件设置，该文件权限出问题会导致报错
            修改权限chmod 4755 /usr/bin/passwd
   * /etc/passwd 存放账户信息
   * /etc/shadow  存放用户密码
   * /etc/group 用户组信息     
* 修改用户所属组

      usermod -g manage wxm #wxm用户组修改为manage
* 删除用户

      #仅删除用户
      userdel username
      #删除用户及home目录下文件
      userdel -r username
* 删除用户组

      groupdel wxm      
* 修改UID

      usermod -u 1015 syf #syf的用户ID修改为1015
* 修改GID



