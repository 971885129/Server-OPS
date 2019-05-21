# slurm 任务调度系统安装

## 安装EPEL Repo
* 如果既想获得 RHEL 的高质量、高性能、高可靠性，又需要方便易用(关键是免费)的软件包更新功能，那么 Fedora Project 推出的 EPEL(Extra Packages for Enterprise Linux)正好适合你。EPEL(http://fedoraproject.org/wiki/EPEL) 是由 Fedora 社区打造，为 RHEL 及衍生发行版如 CentOS、Scientific Linux 等提供高质量软件包的项目。

      yum -y install epel-release
## 安装axel与yum-axelget
*  yum-axelget是EPEL提供的一个yum插件。默认的yum是单线程下载的,使用该插件后用yum安装软件时可以并行下载。

        yum install axel yum-axelget
## 安装与配置NTP服务
* NTP是网络时间协议(Network Time Protocol)，它是用来同步网络中各个计算机的时间的协议。

      yum install ntp -y
      systemctl enable ntpd.service
      ntpdate pool.ntp.org
      systemctl start ntpd

## Munge服务部署和测试
* munge是认证服务，用于生成和验证证书。应用于大规模的HPC集群中。
它允许进程在【具有公用的用户和组的】主机组中，对另外一个【本地或者远程的】进程的UID和GID进行身份验证。
这些主机构成由共享密钥定义的安全领域。在此领域中的客户端能够在不使用root权限，不保留端口，或其他特定平台下进行创建凭据和验证。
简而言之，在集群中，munge能够实现本地或者远程主机进程的GID和UID验证。




