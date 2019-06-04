# Server-operation-and-maintenance

* 自动挂载

      修改/etc/fstab
      /dev/sdi /media/sda auto nosuid,nodev,nofail,x-gvfs-show 0 0
* umount 显示被占用

      #检查占用用户及进程
      fuser -mv /media/sda/

## 用户设置
* 增加用户

* 设置密码

* 修改用户所属组

      usermod -g manage wxm #wxm用户组修改为manage
* 删除用户组

      groupdel wxm
* 修改UID

      usermod -u 1015 syf #syf的用户ID修改为1015

* 修改GID



