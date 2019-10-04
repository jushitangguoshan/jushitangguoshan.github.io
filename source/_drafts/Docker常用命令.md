title: Docker常用命令
tags:
  - Docker
  - 项目管理
categories:
  - 技术分享
keywords: Docker
toc: true
description: '&emsp;&emsp;好记性不如烂笔头，笨鸟就要先飞，更何况不是很笨'
author: 巨石糖果山
date: 2019-10-03 16:30:00
---

    service docker start |stop |restart 启动 停止 重启
    docker run 镜像名称:标签 运行容器 docker -i 交互式操作 docker -t terminal操作
        --rm 退出就删除容器
        --name 指定容器名称
        举例：docker run -it --rm ubuntu:14.04 bash
    docker images 列出已经下载下来的镜像portcommit
        根据仓库名列出镜像 docker images  '仓库名'
        列出特定的某个镜像，也就是说指定仓库名和标签 docker images  '仓库名:标签名'
    docker pull 获取镜像
        docker pull ubuntu:14.04
    docker exec 进入容器
        docker exec -it webserver bash
    docker build [选项] 生成的文件名 上下文(context) 构建镜像
        例如 ： docker build -t nginx:v3 .
    -p <宿主端口>:<容器端口>
    docker rmi [选项] <镜像1> [<镜像2> ...] #docker rm 命令是删除容器，
    docker rm $(docker ps -a -q) 删除所有容器
    
    【docker常见问题】
    Docker CE 镜像源站
        https://yq.aliyun.com/articles/110806?spm=5176.8351553.0.0.d88d105oY5zrn
    Docker 镜像加速器
        https://yq.aliyun.com/articles/29941
    docker mysql设置初始密码(docker mysql启动马上就自动退出)
        docker run 加上环境变量参数 -e MYSQL_ROOT_PASSWORD=password1
    docker redis 设置初始密码
        Dockerfile CMD: 'redis-server --requirepass "password1"'
    docker cron没有执行
        Dockerfile CMD: service cron start
    docker cron 执行的时候时区不对
        RUN echo "Asia/Shanghai" > /etc/timezone
        或者 同步主机时区
        docker run -v /etc/localtime:/etc/localtime <IMAGE:TAG>
        以上两种是网上搜索到的，我都没有成功，我目前的解决方案是将crontab -e配置中的时区都往前推8个小时。
        例如：你本来是要1,9点运行的，设置为1,17
    docker corn 获取不了环境变量
        原因：corn的bash环境变量和docker容器的环境变量不是同一个。
        解决：
        printenv |grep -v "==" | grep -v " " | sed 's/^\(.*\)$/export \1/g' > /project_env.sh \
        && chmod +x /project_env.sh \
        && service cron start \
        && bash
        
        两个grep -v 是为了反正docker-compose link的时候变量变量污染