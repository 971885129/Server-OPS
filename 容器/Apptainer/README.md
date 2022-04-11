# Apptainer

[Apptainer简介]()

### yum

Yum（全称为 Yellow dog Updater, Modified）是一个在Fedora和RedHat以及CentOS中的Shell前端软件包管理器。基于RPM包管理，能够从指定的服务器自动下载RPM包并且安装，可以自动处理依赖性关系，并且一次安装所有依赖的软件包，无须繁琐地一次次下载、安装。

#### yum.conf文件

etc/yum.conf文件是用来存储yum配置信息的文件，虽然yum.conf文件通常都比较简洁，却是yum软件管理器的重要组成部分。

#### repo文件

etc/yum.repos.d/目录下的众多.repo文件，repo文件是yum源（软件仓库）的配置文件，通常一个repo文件定义了一个或者多个软件仓库的细节内容，例如我们将从哪里下载需要安装或者升级的软件包，repo文件中的设置内容将被yum读取和应用。

#### yum工作原理

执行yum命令时，会首先从”/etc/yum.repo.d”目录下的众多repo文件中取得软件仓库的地址并下载“元数据”，“元数据”包含注册于该软件仓库内所有软件包的包名及其所需的依赖环境等信息，yum得到这些信息后会和本地以后环境做对比，进而列出确认需要安装哪些包，并在用户确认后开始安装。

#### yum源

EPEL

高质量、高性能、高可靠性，方便易用(关键是免费)的软件包更新功能，由 Fedora 社区打造，为 RHEL 及衍生发行版如 CentOS、Scientific Linux 等提供高质量软件包的项目。

	# 安装epel源
	yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
	# 安装yum-utils
	yum install yum-utils
	# 这在做什么？
	yum-config-manager --enable "rhel-*-optional-rpms"
	# 清除旧软件包缓存
	yum clean all
	# 重新生成缓存
	yum makecache

#### yum更新

	yum update -y

* yum update 会升级所有包、软件和系统内核，一般仅在新装系统时使用，日常不推荐
* yum upgrade 只升级所有包忙不升级软件和系统内核



### 创建自定义容器

基础镜像

	# 创建centos7容器
	apptainer build --sandbox centos7_R4.0_sandbox docker://centos:centos7


























