# Conda使用

[Conda简介](https://mubu.com/app/edit/home/5o_XaCuKeM0)


### Conda安装后配置

Anaconda 仓库与第三方源的国内镜像配置
[清华源](https://mirrors.tuna.tsinghua.edu.cn/help/anaconda/)


### Conda环境迁移

clone

	# 创建环境的快照或备份
	conda create --name snapshot --clone myenv # 直接复制本机环境

spec list

	# 导出环境中安装的包及url
	conda list --explicit > spec-list.txt

	# 创建新环境
	conda create --name python-course --file spec-list.txt

environment.yml

	# 导出环境
	conda env export > environment.yml

	# 创建新环境
	conda env create -f environment.yml

Conda pack

打包环境

	# Pack environment my_env into my_env.tar.gz
	conda pack -n my_env

	# Pack environment my_env into out_name.tar.gz
	conda pack -n my_env -o out_name.tar.gz

	# Pack environment located at an explicit path into my_env.tar.gz
	conda pack -p /explicit/path/to/my_env

重现环境

	# Unpack environment into directory `my_env`
	mkdir -p my_env
	tar -xzf my_env.tar.gz -C my_env

	# Use Python without activating or fixing the prefixes. Most Python
	# libraries will work fine, but things that require prefix cleanups
	# will fail.
	./my_env/bin/python

	# Activate the environment. This adds `my_env/bin` to your path
	source my_env/bin/activate

	# Run Python from in the environment
	(my_env) $ python

	# Cleanup prefixes from in the active environment.
	# Note that this command can also be run without activating the environment
	# as long as some version of Python is already installed on the machine.
	(my_env) $ conda-unpack

#### 总结

* Spec List方法只能在相同系统中重现环境，且pip安装的软件包不包含在内
* environment.yml方法由于只包含软件包名称，可在不同系统中重现环境，并且包括使用pip安装的软件包
* Conda Pack方法只能在同样操作系统的计算机之间迁移，包括了环境中安装的软件包和二进制文件，适合在无法联网的设备中重现环境

#### 注意

conda的环境并不是完全隔离，仍会对系统有一定依赖(例如部分动态库的依赖)

#### 问题

* 是否可手动安装软件到指定环境中

























