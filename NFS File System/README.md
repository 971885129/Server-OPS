# NFS 文件系统

## 服务端
* 关闭防火墙

* 安装NFS服务

      yum -y install nfs-utils rpcbind
* 配置文件设置

      vi /etc/exports
      /media 172.16.0.0/24(rw,no_root_squash,no_all_squash)
* 启动服务

      service nfs start && chkconfig nfs on


## 客户端挂载

    mount.nfs 172.16.0.95:/media /media

## 备注
* sync参数可能影响数据写入速度，大数据可设置async(待测试）


