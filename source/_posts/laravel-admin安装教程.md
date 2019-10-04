---
title: laravel-admin教程之安装（爬坑系列一）
date: 2019-08-22 22:54:48
tags: [laravel-admin, php后台框架]
categories: php开发
keywords: laravel-admin
toc: true
description: 
---

&emsp;&emsp;laravel-admin是一个可以快速帮你构建后台管理的工具，它提供的页面组件和表单元素等功能，
能帮助你使用很少的代码，就实现功能完善的后台管理功能。
<span style="font-size:0.8rem; background:yellow;">
    特别注意：当前laravel-admin版本(v1.7.+)需要安装PHP 7+和Laravel 5.5+
</span>


<font style="text-align:center;font-size:1rem;text-align:center;">
    <center>Θ废话不多说，直接开爬</center>
</font>

#### 1、搭建安装环境：安装composer
&emsp;&emsp;如果没有安装composer请百度安装composer，此处默认已经安装composer,然后
新建一个文件夹，名字任意取，此处文件夹名称为laravel_demo,然后进入该文件夹。
##### 2、安装laravel框架：
        --------在项目文件下的cmd窗口，执行下面命令-------

        #1)首先：使用 Composer 下载 Laravel 的安装程序：
            composer global require "laravel/installer"

        2)接着用laravel 用安装器这样执行一句命令(建一个名字叫laravel-admin的laravel框架项目)：
            laravel new laravel-admin
        3)在根目录下的.env文件里面把数据库信息填写好建完之后,此时就已经是一个完整的laravel项目了，
        
##### 3、正式安装laravel-admin：

     1)此时仍然是在项目文件下的cmd窗口，执行下面命令：
        composer require encore/laravel-admin
        
    2)成功后继续执行下面的命令来发布资源：
        php artisan vendor:publish --provider="Encore\Admin\AdminServiceProvider"
        
    3)成功后继续执行：
        php artisan admin:install
        
&emsp;&emsp;执行到这里你就应该掉坑里面了，因为这个坑我也爬过，但是爬了一些不必要的时间，下面我就让你们正确一次
性过这个坑(记得留言点个赞，爬坑不容易)
           
      若出现错误提示：SQLSTATE[42000]: Syntax error or access violation: 1071 Specified key was too long;
                    max key length is 1000 bytes (SQL: alter tableusersadd uniqueusers_email_unique(email))       
      
      解决办法：在app\Providers\AppServiceProvider.php添加默认值
             <?php
                    namespace App\Providers;
                    use Illuminate\Support\ServiceProvider;
                    use Illuminate\Support\Facades\Schema;
                    class AppServiceProvider extends ServiceProvider
                    {
                        /**
                         * Bootstrap any application services.
                         * @return void
                         */
                        public function boot()//在这个方法里面加上这么一句就ok（完美来起）
                        {
                            Schema::defaultStringLength(191); //   <--就是这一句
                        }
                    }
             ?>
&emsp;&emsp;接下来就可以启动服务了，（此处作为一个laravel新手就不知道什么叫启动服务了，实不相瞒，我也是，不
过幸好你遇到我）
        
            执行一下命令启动服务：php artisan serve
&emsp;&emsp;接着你就可以打开http://127.0.0.1:8000来访问laravel-admin了

&emsp;&emsp;运行到此处就已经来起了（不过后面坑也跟着来了）

        1)进入laravel-admin后台：http://127.0.0.1:8000/admin/auth/login，初始账户and密码都为admin
        
        2)如果报错的话，很正常，因为我也报了很久的错（你们真好，一次性就能解决，我可是爬了很久的坑）
            错误提示;SQLSTATE[HY000] [1045] Access denied for user 'root'@'localhos'......
            解决办法：
                如果是本地运行phpstudy跑项目，那么直接进入mysql管理把账户密码都改为你的.env里面配置的密码
                服务器上同上面的操作一样
##### 然后大功告成！（去闯吧少年）


        

    
 
