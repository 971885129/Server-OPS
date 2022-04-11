# Apptainer

[Apptainer简介](https://mubu.com/app/edit/home/6U5gCFKG800)

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
* yum upgrade 只升级所有包而不升级软件和系统内核


### 创建自定义容器

#### 基础容器创建

	# 创建centos7容器
	apptainer build --sandbox centos7_R4.0_sandbox docker://centos:centos7

#### 基础容器环境配置

	# yum升级
	yum update -y
	# yum源配置
	yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
	yum install yum-utils
	yum-config-manager --enable "rhel-*-optional-rpms"
	yum clean all
	yum makecache
	# vim安装
	yum -y install vim
	# less安装
	yum -y install less

	# R Python依赖

	# openssl安装
	yum install openssl openssl-devel
	# libxml安装
	yum install libxml2 libxml2-devel
	# Development Tools
	yum groupinstall "Development Tools" -y
	# libffi bzip2安装
	yum install libffi-devel bzip2-devel -y
	# ncurses
	yum install ncurses-devel -y

#### 安装R

	# 下载
	curl -O https://cdn.rstudio.com/r/centos-7/pkgs/R-4.0.0-1-1.x86_64.rpm
	# 安装
	yum install R-4.0.0-1-1.x86_64.rpm
	# 安装位置/opt/R/4.1.0/bin/R

#### 安装Python

	# 下载
	wget https://www.python.org/ftp/python/3.8.0/Python-3.8.0.tgz
	# 安装
	tar xvf Python-3.8.0.tgz && \
	cd Python-3.8.0/ && \
	./configure --enable-optimizations && \
	make altinstall
	# pip更新
	/usr/local/bin/python3.8 -m pip install --upgrade pip

	# 基础模块安装
	/usr/local/bin/pip3.8 install readline
	/usr/local/bin/pip3.8 install gnureadline

#### 总结

* 若所在当前目录路径与容器内一致，那么运行容器时，容器内所处位置及为当前路径，可能与容器读取宿主机环境变量有关
* 容器会默认挂载用户home目录，且运行容器的默认位置也是在home目录


#### 问题

* 尚不清楚如何修改容器启动时的默认目录
* R4.1使用GEOquery下载数据会中断，改为使用R4.0
* 修改容器内/etx/profile无法设置容器内环境变量，需要通过/.singularity.d/env/90-environment.sh修改

容器内conda使用
* 安装完conda后，无法conda init,无法使用conda activate 激活容器
* 使用source activate 可激活容器，但无法完成软件安装，会报错找不到conda；而且使用该方法进入下载的包含R的conda容器，启动R会报错

		package or namespace load failed for ‘utils’:
* 直接下载r4.1容器镜像，创建容器进入R后，安装包提示缺少动态库，但该镜像又没有yum和rpm

鉴于以上conda相关问题，采用centos7为基础镜像自定义容器

#### 参考

	# R
	https://docs.rstudio.com/resources/install-r/#optional-install-recommended-packages
	# Python
	https://computingforgeeks.com/install-latest-python-on-centos-linux/
	https://blog.csdn.net/shitou_12/article/details/119679790
	# 其他
	https://apptainer.org/docs/user/main/quick_start.html#quick-installation-steps
	https://pythonspeed.com/articles/activate-conda-dockerfile/























