# Docker

虚拟化
	将实体资源虚拟

Docker组件
	cs
	Docker client
	Docker server（守护进程）
	Docker 容器
	image
		基于镜像运行自己的容器（容器模板）
	Registry
		存储镜像
	
Docker命令
	镜像
		查看镜像
			docker images
		搜索镜像
			docker search xxx
			AutoMated:
				自动构建，表示该镜像是由docker hub自动构建流程创建的
		拉取镜像
			docker pull xxx
		删除镜像
			docker rmi xxxid
	容器
		通过镜像运行
		查看容器
			docker ps
				-a所有
				-l上次
		创建与启动
			docker run
				-i运行
				-t启动后进入其命令行
				--name 为容器命名
				-v 表示目录映射关系（宿主机-容器机）
				-d 守护式运行（后台）（不会登录容器）
				-p 表示端口映射
				交互式
					docker run -it --name=xxx repository:TAG /bin/bash（运行的命令），（加载centos命令行）
					exit：退出（退出即停止）
				守护式
					docker run -di --name=xxx repository:TAG 
					docker exec -it xxx /bin/bash 进入容器
					docker stop xxx/xxxid
					docker start xxx/xxxid
				文件拷贝
					docker cp 文件路径 xxx：文件路径
					docker cp xxx:文件路径 文件路径
				目录挂载
					docker run -di -v 宿主机目录:容器目录 --name=xxx repository：TAG
				查看容器ip地址
					docker inspect xxx
					//NetWorkSetting gateway
					docker inspect --format=`{{.xx.xx}}` xxx
				删除容器
					docker rm xxx
				mysql部署
					docker run -di --name=xxxxx -p 33306:3306 -e MYSQL_ROOT_PASSWORD=xxxxxx xxx
	

## 迁移与备份
​	容器保存为镜像
​		docker commit mynginx mynginx_i
​	镜像备份
​		docker save -o mynginx.tar mynginx_i
​	镜像恢复与迁移
​		docker load -i mynginx.tar
## DockerFile
​	基于一个基础镜像构建一个新镜像的 命令及参数构成的脚本
​		FROM images——name：tag
​			使用的基础镜像
​		MAINTAINER user_name 
​			声明镜像的创建者
​		ENV key value
​			设置环境变量
​		RUN command
​			DockerFile命令
​		ADD source_dir/file dest_dir/file
​			自动解压复制
​		COPY source_dir/file dest_dir/file
​			自动复制
​		WORKDIR path_dir
​			设置工作目录（命令基础//相对路径）
​			
​		DockerFile必须为这个名字
​		
​		docker build -t=‘镜像名’ .（指定当前目录）
## Docker私有仓库
​	docker pull registry
​		拉取私有仓库镜像
​	docker run -di --name=registry -p 5000：5000 registry
​		启动私有仓库容器
​	ip：5000/v2/_catalog
​		{
​			"repositories"
​		}
​	私有仓库信任
​		vi /etc/docker/daemon.json
​		添加
​			"insecure-registries":["ip:5000"]
​		systemctl restart docker
​	上传至私有仓库
​		docker tag 【repository】 ip：5000/【repository】
​		push ip：5000/【repository】


​		
​		
​		
​		
​		
​		
​		