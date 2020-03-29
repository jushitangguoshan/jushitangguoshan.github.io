---
title: Docker之搭建项目环境
date: 2019-09-15 18:02:56
tags: [Docker, ]
categories: [项目管理]
keywords: [Docker之搭建项目环境, ]
toc: true
description: "&emsp;&emsp;本篇博文主要是以搭建一个linux+php+nginx+mysql+redis环境教程"
---

docker run -v D:\myphp_project\docker_project:/data/myphp_project/docker_project -p 9501:9501 -it --entrypoint /bin/sh hyperf/hyperf:7.2-alpine-cli
