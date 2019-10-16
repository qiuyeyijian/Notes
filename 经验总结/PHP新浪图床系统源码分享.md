# PHP新浪图床系统源码分享

## 在这篇文章中：

- [系统介绍](javascript:;)
- [安装](javascript:;)
- [环境条件](javascript:;)
- [预览](javascript:;)
- [常见问题解决](javascript:;)

###  	系统介绍 

​     在幻想领域中, 图床图片全部托管在 新浪云, 每张图片都有多张不同级别的缩略图.这便是幻想领域的最大特色之一.

 拥有较为完善的用户系统与管理员系统。管理员在后台拥有完全权限，对网站的一切基本配置

​     我的图库，将会罗列出用户自己所上传的所有图片，管理员则显示系统托管的所有图片.你可以在这里对图片进行删除、预览或者复制它，但删除仅仅只是不再出现在本系统中，图片仍然是存在于新浪之上，这点你是要知道的.

​     探索，它是前台对用户图片预览的功能，在这里你可以发现和找到你需要的东西.如果你不需要它，可以在后台进行关闭设置.

​     上传新浪图床并非无要求，它需要你进行登录验证，但我们拥有一套独立的新浪登录程序，不依赖任何扩展，并且无验证码，cookie过期将自动为你进行登录，为你解决一切后顾之忧，所以你必须在后台设置你的新浪账号密码才能正常使用.

###  	安装 

​     你需要将幻想领域的源代码解压缩并上传至网站根目录，访问网站域名会自动跳转到安装程序，根据向导提示安装即可。如果未跳转，请手动访问http://您的域名/install.php 进行安装

​     首次安装成功后需要登录管理员后台对图床进行一些基本配置，才能使用

 	    后台地址：http://您的域名/admin 但是讽刺的是，您需要在前台进行登录 

###  	环境条件 

 	    请注意，幻想领域自1.0版本起只支持PHP版本≥5.6<7.1，请注意更新您的PHP版本。 

 	    需要伪静态规则支持 

 	Apache： 

```javascript
<IfModule mod_rewrite.c>
RewriteEngine on
RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME} !-f
Rewritebase /index.php
RewriteRule ^(.*)$ /index.php?/$1 [L]
</IfModule>
```

 Nginx： 

```javascript
if (!-d $request_filename){
	set $rule_0 1$rule_0;
}
if (!-f $request_filename){
	set $rule_0 2$rule_0;
}
if ($rule_0 = "21"){
	rewrite ^/(.*)$ /index.php?/$1 last;
}
```

###  	预览 

![img](https://ask.qcloudimg.com/http-save/yehe-1654742/1ifoqe3w1f.jpeg?imageView2/2/w/1620)

###  	常见问题解决 

 	1、IP获取不真实 

 	    找到路径/framework/helpers/function.base.php第111行替换整个getip 

```javascript
function getIp() {
  global $_SERVER;
  if (getenv('HTTP_CLIENT_IP')) {
    $ip = getenv('HTTP_CLIENT_IP');
  } else if (getenv('HTTP_X_FORWARDED_FOR')) {
    $ip = getenv('HTTP_X_FORWARDED_FOR');
  } else if (getenv('REMOTE_ADDR')) {
    $ip = getenv('REMOTE_ADDR');
  } else {
    $ip = $_SERVER['REMOTE_ADDR'];
  }
  return $ip;
}
```

 	2、后台数据请求异常 

 	    找到路径/framework/core/Framework.php第51行到第53行注释或者删除，具体代码如下 

```javascript
if ($path != '') {
           $path = strstr(trim($_SERVER['REQUEST_URI'],'/'),$path);
}
```

 	3、163邮箱发信失败 

 	    25号端口应该是被封了，如果不能开启那就切换端口 

 	    找到路径framework/libraries/phpmail/Smtp.class.php第29行$smtp_port = 25修改成$smtp_port = 465或者另外的端口即可 

 	4、验证码不显示 

 	    应该是你的伪静态没有设置成功，请参考前面的环境条件进行设置 

本文参与[腾讯云自媒体分享计划](https://cloud.tencent.com/developer/support-plan)，欢迎正在阅读的你也加入，一起分享。

发表于 2018-06-06