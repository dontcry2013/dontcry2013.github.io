---
title: 函数指针和指针函数
date: 2016-04-29 18:34:12
categories: basic
tags: c
---

开启Apache：sudo apachectl start
关闭Apache：sudo apachectl stop
重启Apache：sudo apachectl restart



开启PHP
　　开启PHP，需要修改Apache配置文件，方法如下：

打开终端，输入命令：sudo vim /etc/apache2/httpd.conf
找到#LoadModule php5_module libexec/apache2/libphp5.so，去掉注释（删除前面的井号）。
　　mac下Apache的默认文件夹为/Library/WebServer/Documents，在该目录下创建一个名为index.php文件，在文件中添加如下内容：<?php phpinfo(); ?>。删除原目录下的index.html文件，然后在浏览器中输入localhost，如果出现如下PHP的info页，则表示PHP开启成功，如果不成功，用前面的命令重启Apache再试。

文／zagger（简书作者）
原文链接：http://www.jianshu.com/p/2fb9a3bb12f6
著作权归作者所有，转载请联系作者获得授权，并标注“简书作者”。




修改Apache目录
　　上面说到了mac下Apache的默认文件夹为/Library/WebServer/Documents，该目录默认是隐藏的，操作不是很方便，我们可以将其修改成自定义的目录。

打开终端，输入命令：sudo vim /etc/apache2/httpd.conf
找到如下两处
　　DocumentRoot "/Library/WebServer/Documents"
　　<Directory "/Library/WebServer/Documents">
将两处中引号中的目录替换为自定义的目录
　　完成以上三步后，重启Apache，将之前创建的index.php文件拷贝到自定义目录中，然后在浏览器中输入localhost，如果出现PHP的info页，则表示目录修改成功

文／zagger（简书作者）
原文链接：http://www.jianshu.com/p/2fb9a3bb12f6
著作权归作者所有，转载请联系作者获得授权，并标注“简书作者”。
