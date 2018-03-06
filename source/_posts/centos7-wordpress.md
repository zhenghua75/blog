---
title: centos7 wordpress
date: 2018-02-28 21:57:47
tags:
---

```shell
更新环境
[root@VM_0_14_centos ~]# yum update -y
安装httpd、php、mysql
[root@VM_0_14_centos ~]# yum install httpd -y
[root@VM_0_14_centos ~]# yum install php php-mysql -y 
[root@VM_0_14_centos ~]# yum install mariadb-server -y
```
<!-- more -->
```shell
[root@VM_0_14_centos ~]# systemctl start httpd
[root@VM_0_14_centos ~]# systemctl enable httpd
[root@VM_0_14_centos ~]# echo "This is a httpd test page by zhenghua.">/var/www/html/index.html
[root@VM_0_14_centos ~]# systemctl start mariadb.service
[root@VM_0_14_centos ~]# systemctl enable mariadb.service
[root@VM_0_14_centos ~]# echo " 
<html>
<h1>This is a php test page.</h1>
<?php
phpinfo();
?>
</html>">/var/www/html/index.php
[root@VM_0_14_centos ~]# vi /var/www/html/connmysql.php
<?php
        $conn = mysql_connect('127.0.0.1','root','');
        if ($conn)
                echo "succ";
        else
                echo "failure";

        mysql_close();
?>
安装wordpress
[root@VM_0_14_centos ~]# cd /usr/local/src
[root@VM_0_14_centos ~]# wget https://cn.wordpress.org/wordpress-4.9.4-zh_CN.tar.gz
[root@VM_0_14_centos ~]# tar -xf wordpress-4.9.4-zh_CN.tar.gz
[root@VM_0_14_centos ~]# cp -R wordpress /var/www/html/
[root@VM_0_14_centos ~]# cd /var/www/html/wordpress
[root@VM_0_14_centos ~]# cp wp-config-sample.php wp-config.php
[root@VM_0_14_centos ~]# vi wp-config.php
[root@VM_0_14_centos ~]# mysql

MariaDB [(none)]> create database wpdb character set utf8;
MariaDB [(none)]> grant all privileges on wpdb.* to 'wpadmin'@'localhost' identified by 'pass';
安装ftp
[root@VM_0_14_centos ~]# yum -y install vsftpd
[root@VM_0_14_centos ~]# systemctl start vsftpd.service
[root@VM_0_14_centos ~]# systemctl enable vsftpd.service
[root@VM_0_14_centos ~]# vi /etc/vsftpd/vsftpd.conf

#修改
anonymous_enable=NO    #禁止匿名访问
data_connection_timeout=5    #数据连接超时时间。如果在使用vsftpd上传下载碎小文件的时候容易发生超时中断的问题，将120改成5或者更小为佳
#使ftp用户始终在项目部署的根目录范围内，禁止访问系统其它文件
chroot_local_user=YES    #打开注释
chroot_list_enable=YES    #打开注释
chroot_list_file=/etc/vsftpd/chroot_list    #打开注释
allow_writeable_chroot=YES    #添加
#默认保持不变
xferlog_enable=YES    #启用上传和下载的日志功能。它可以对用户的操作进行日志记录，当出现问题的时候可以通过日志排查问题
#设定支持ASCII模式的上传和下载功能
ascii_upload_enable=YES
ascii_download_enable=YES 
[root@VM_0_14_centos ~]# vi /etc/vsftpd/chroot_list

[root@VM_0_14_centos ~]# useradd -d /var/www/html -s /sbin/nologin ftpuser
[root@VM_0_14_centos ~]# passwd ftpuser

[root@VM_0_14_centos ~]# chmod -R 777 /var/www/html/
升级完成
[root@VM_0_14_centos ~]# chmod -R 644 /var/www/html/

```