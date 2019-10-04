---
title: Docker之安装
date: 2019-09-09 22:18:29
tags: [Docker, 项目管理]
categories: 技术分享
keywords: Docker
toc: true
description: "&emsp;&emsp;最近刚接触到docker，所以就边折腾边记录这个流程，便于自己或者他人参考，其中有可能会有些偏差，但都八九不离十，要声明一下，我这是第一次安装Docker，所以沿途遇到的美景都比较多，走的路也可能稍微弯了那么一点点，但还算好，结果跟过程一样在期望中，所以那就直接开始吧！"
---
<script type="text/javascript" src="/js/src/bai.js"></script>
&emsp;&emsp;前提说明：Docker for Windows是一个Docker Community Edition（CE）应用程序。Docker for Windows安装包包
含了在Windows系统上运行Docker所需的一切。如果你不想装虚拟机，想直接在你的Windows操作系统中安装与学习使用docker，那么
你首先得查看你的系统是否满足Docker for Windows的安装与使用要求。
## Windows 中Docker安装应用
### 安装前准备
#### &emsp;&emsp;简单介绍：
   &emsp;&emsp;Docker for Windows的当前版本运行在64位Windows10 Pro，专业版、企业版和教育版（1607年纪念更新，版本14393或更高版本）
   上。（Ps:家庭版是不行的，如果你是家庭版，那么一是升级到专业版，破解专业版推荐个地址：http://blog.csdn.net/SONGCHUNHON
   G/article/details/78006389，二是安装Docker Toolbox，自行百度）
   </br>
#### &emsp;&emsp;安装条件：
   &emsp;&emsp;如果你满足Docker for Windows的环境条件了，那么首先检查电脑的虚拟化开启了没有：进入任务管理器（ctrl+alt+delete），
   点击性能->cpu ,查看虚拟化是否已启用，如果虚拟化是已禁用，那么你需要重启电脑进入bios开启虚拟化（我们一般的笔记本cpu都
   是支持虚拟化的，重启时进入bios按esc -> 再按f12 -> 去开启虚拟化，你得开机进入BIOS里把Virtualization的选项变成Enabled
   （不同机子的Virtualization可能在不同的选项下，去看一下advanced之类的））。
 
### 开始安装
#### &emsp;&emsp;检测cpu虚拟化
   &emsp;&emsp;开启虚拟化重启后，打开任务管理器，点击cpu查看虚拟化是否已启用
{% qnimg 查看是否开启虚拟化.jpg title:查看是否开启虚拟化  'class:class1 class2' extend:?imageView2/2/w/1000 %}

#### &emsp;&emsp;Hyper配置
   &emsp;&emsp;入电脑的控制面板->程序->启用或关闭Windows功能->把Hyper-v勾上，启用后电脑会重启，后面就可以下载并安装
                Docker for Windows了。
{% qnimg windows勾选hyper.jpg title:windows勾选hyper  'class:class1 class2' extend:?imageView2/2/w/1000 %}

### 下载docker for windows
#### &emsp;&emsp;下载并安装
   &emsp;&emsp;&emsp;进入网址https://docs.docker.com/docker-for-windows/install/#download-docker-for-windows 下载并安装,
                安装过程直接下一步下一步就Ok了。
#### &emsp;&emsp;注册登录启动
   &emsp;&emsp;&emsp;启动以后会出现在桌面的右下角区域，鼠标放上去以后显示Docker is running表示启动成功，第一次安装启用好像
                是会弹出个Docker Cloud登录界面，去注册然后登录，使用和git有点类似，可以pull图像等等 
{% qnimg 登录docker.jpg title:登录docker  'class:class1 class2' extend:?imageView2/2/w/1000 %}
   
### Docker使用前先测试环境
#### &emsp;&emsp;检测检查Docker，Compose和Machine的版本
   &emsp;&emsp;&emsp;进入cmd窗口，检查Docker，Compose和Machine的版本，输入以下命令，不报错即可。
{% qnimg docker使用前检测.jpg title:docker使用前检测  'class:class1 class2' extend:?imageView2/2/w/1000 %}
#### &emsp;&emsp;确保docker命令正常工作
   <pre>输入cmd命令：docker ps</pre>
#### &emsp;&emsp;如果报以下错误（先稍等几十秒，有可能是docker正在启动）：
{% qnimg docker检测版本报错_1.jpg title:docker检测版本报错_1  'class:class1 class2' extend:?imageView2/2/w/1000 %}  
#### &emsp;&emsp;解决方法：
     一、确保开启cpu虚拟化、确认Hyper-V启动(上面Part1中2、3步)
     
     二、进入cmd执行下面两行命令:
        cd "C:\Program Files\Docker\Docker"
        ./DockerCli.exe -SwitchDaemon
        注：如果发现执行不了，那么你可以在windows左下角搜索PowerShell来执行
#### &emsp;&emsp;解决后再试一次：
   <pre>输入cmd命令：docker ps</pre>
#### &emsp;&emsp;出现下面内容则成功：
   {% qnimg 解决docker_ps.jpg title:解决docke_ps报错问题  'class:class1 class2' extend:?imageView2/2/w/1000 %} 
   
### 开始测试   
#### &emsp;&emsp;从Docker Hub中拉取图像并启动容器 
  <pre>在windows PowerShell输入cmd命令：docker ps</pre>
#### &emsp;&emsp;将会出现以下内容，等待下载完成即可
{% qnimg 测试从Docker_Hub中拉取图像并启动容器.jpg title:运行docker-run-hello-world以测试从Docker-Hub中拉取图像并启动容器   'class:class1 class2' extend:?imageView2/2/w/1000 %} 
#### &emsp;&emsp;接着成功之后出现以下内容：
{% qnimg docker_hub测试完成后内容.jpg title:运行docker-run-hello-world以测试从Docker-Hub中拉取图像并启动容器完成后内容   'class:class1 class2' extend:?imageView2/2/w/1000 %} 

### 运行一个Ubuntu容器
#### &emsp;&emsp;在windows PowerShell输入cmd命令：
{% qnimg 运行一个Ubuntu容器.jpg title:运行一个Ubuntu容器  'class:class1 class2' extend:?imageView2/2/w/1000 %} 
#### &emsp;&emsp;启动Ubuntu容器成功之后
{% qnimg 启动Ubuntu容器成功之后.jpg title:启动Ubuntu容器成功之后  'class:class1 class2' extend:?imageView2/2/w/1000 %} 
#### &emsp;&emsp;我们先结束这个容器
   <pre>在Ubuntu 容器里面直接输入： exit;</pre>
#### &emsp;&emsp;启动一个webserver nginx 服务
    在windows PowerShell输入cmd命令：docker run -d -p 80:80 --name webserver nginx 
    注：这一步时间可能有点长，耐心稍等！
    （特别注意：网上大部分教程有把'... --name...'写成：'... -name...'，简直坑死了）
&emsp;&emsp;启动一个Dockerized webserver 会下载nginx容器图像并启动它，然后再打开浏览器
{% qnimg 启动webserver_nginx并检查状态.jpg title:启动webserver_nginx并检查状态  'class:class1 class2' extend:?imageView2/2/w/1000 %} 
&emsp;&emsp;访问http://localhost时出现这个界面说明已经成功了
{% qnimg 启动webserver_nginx成功界面.jpg title:启动webserver_nginx成功界面  'class:class1 class2' extend:?imageView2/2/w/1000 %} 
##### 然后大功告成！（去闯吧少年）  
    
    
    