---
title: 换电脑后如何继续写博客-hexo
tags: [hexo, 博客]
categories: [博客]
keywords: Hexo、继续写博客、重装电脑
toc: true
description: >-
  &emsp;&emsp;平时有写博客习惯的搬运工不知道有没有这么一个苦恼，如果你的博客是通过博客工具
  搭建的话，那么只能在你最初搭建的那台电脑上进行设置、编辑博客，如果某一天那台电脑系统崩了，或者重装系统了
  或者你想在其他电脑上去编辑的话就很麻烦，所以本篇博文针对Hexo-github/coding来进行博客备份，完成以下流程可以
  实现多终端编辑博客，再也不用担心当我想记一下博客的时候确不知道来写（此流程只能在你最开始搭建博客那台 终端上操作）。
date: 2019-10-11 11:34:01
---
<script type="text/javascript" src="/js/src/bai.js"></script>
    
# 在博客的GitHub仓库新建一个分支hexo
   &emsp;&emsp;在红色箭头处输入一个新的分支名称，以下就是我添加好的分支展示情况
   {% qnimg github新建分支hexo.jpg title:github新建分支hexo  'class:class1 class2' extend:?imageView2/2/w/1000 %}
    &emsp;&emsp;这样配置的话在GitHub上的http://yourgithubname.github.io仓库就有两个分支，一个hexo分支用来存放网站的原始文件，一
    个master分支用来存放生成的静态网页
# 将hexo分支git clona到一个文件夹(new_git_blog_file)中
   &emsp;&emsp;此时文件夹是空的，然后将你的博客根目录下中_config.yml、theme、source、scaffolds、package.json、.gitignore文件复制到new_git_blog_file下
   <br/>
   &emsp;&emsp;这样一来，在GitHub上的http://yourgithubname.github.io仓库就有两个分支，一个hexo分支用来存放网站的原始文件，一个master分支用来存放生成
   的静态网页，这里必须**注意，在你复制过来的文件夹下面有可能含有.git文件，需要删除后你才能push**，比如theme/next文件夹下的.git文件。删除
   时需要在‘我的电脑’里面打开然后删除，然后就在这个新文件夹下(new_git_blog_file)中执行
        
    git .add ./
    git commit -m 'hexo code'
    git push
   &emsp;&emsp;提交到hexo分支
# 另一台电脑上重新安装博客环境
   
    1、首先安装git 、nojs
    2、然后新建一个文件夹比如：myblog
    3、在myblog文件下git clone 你的GitHub仓库里面博客源码的hexo分支
    4、然后进入yourblogname.github.io文件夹下面，安装hexo以及相关模块
    5、依次执行 npm install hexo --save 、npm install 、npm insatll hexo-deployer-git   
   &emsp;&emsp;然后在myblog目录中就可以执行命令 hexo s查看是否配置成功
# 新装的博客环境必要操作
   &emsp;&emsp;在新装的博客环境中，所在分支是在hexo，而我们博客的配置是设置在master分支上的，所以当写完一篇博客后，除了执行发布命令
   
    hexo clean && hexo g -d
   &emsp;&emsp;还需要将博客配置更新到hexo 分支，此时则可以使用git命令提交就行
    
    git .add ./
    git commit -m 'hexo code'
    git push
  &emsp;&emsp;至此以后再每个不同终端上面想要编辑博客时，都要先Git pull origin hexo的配置，完了之后再hexo g -d ***并且git push orgin hexo
  提交配置文件部分到hexo分支***
