# Server-operation-and-maintenance

* 自动挂载

      修改/etc/fstab
      /dev/sdi /media/sda auto nosuid,nodev,nofail,x-gvfs-show 0 0
* umount 显示被占用

      #检查占用用户及进程
      fuser -mv /media/sda/


