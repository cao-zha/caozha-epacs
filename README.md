# caozha-CEPCS（新冠肺炎疫情防控系统）

caozha-CEPCS，是一个基于PHP开发的新冠肺炎疫情防控系统，CEPCS（全称：COVID-19 Epidemic Prevention and Control System），可以应用于单位、企业、学校、工业园区、村落等等。小小系统，希望能为大家渡过疫情尽自己微薄之力。

### 功能介绍

**前端功能**

前端功能分为：员工（访客）登记与登陆、我的资料、我的二维码（有管理权限的人登陆后扫用户提供的二维码可以直接查看此用户的所有信息）、疫情上报、疫情公告等模块，以实现对企业或园区内部进行高效的疫情管控。

前端登陆是基于身份证号和密码进行登陆验证，所以，注册时或者后台添加会员时，会先验证身份证号是否已存在系统里，如已存在则提示不能注册。

安全方面，系统对入库数据做了必要的过滤；生成的二维码也做了加密验证处理，防止恶意用户伪造二维码。


**后端功能**

后端基于开源免费的[caozha-admin](https://gitee.com/dengzhenhua/caozha-admin)架构开发，功能完善，有：疫情新闻公告、会员管理、疫情上报记录、系统设置、管理员维护、权限组管理、系统日志等等功能。


更多功能，自己下载体验吧。


### 安装方法

**快速安装**

1、PHP版本必须7.1及以上。

2、上传目录/Src/内所有源码到服务器。

3、设置网站的根目录指向运行目录/public/。（此为ThinkPHP6.0的要求）

4、将/Database/目录里的.sql文件导入到MYSQL数据库。

5、修改文件/config/database.php，配置您的数据库信息。

6、后台访问地址：http://您的域名/index.php/admin/index/login   (账号：caozha   密码：123456)

7、前端访问地址：http://您的域名/index.php   (测试账户：450881000000000011  密码：123456)


**伪静态设置**

1、ThinkPHP框架必须在运行目录下设置伪静态才能正常访问，否则会显示404错误。

2、如果您使用的是Apache，伪静态设置为（.htaccess）：

<IfModule mod_rewrite.c>

  Options +FollowSymlinks -Multiviews
  
  RewriteEngine On
  
  RewriteCond %{REQUEST_FILENAME} !-d
  
  RewriteCond %{REQUEST_FILENAME} !-f
  
  RewriteRule ^(.*)$ index.php?s=$1 [QSA,PT,L]
  
</IfModule>


3、如果您使用的是Nginx，伪静态设置为：

location / {

    index index.php;
    
    if (!-e $request_filename) {
    
       rewrite  ^(.*)$  /index.php?s=/$1  last;
       
       break;
       
    }
    
}


4、在网站运行目录（/public/）下，有两个文件：.htaccess和nginx.htaccess，分别是Apache和Nginx的伪静态文件，您可以直接拿来使用。

 
**开发手册**

***后端：***

采用开源免费的[caozha-admin](https://gitee.com/dengzhenhua/caozha-admin)架构，安装和使用方法也跟caozha-admin类似，所以请参考Wiki：

国内：https://gitee.com/dengzhenhua/caozha-admin/wikis

国外：https://github.com/dengcao/caozha-admin/wiki

***前端：***

采用网上免费下载的模板制作，如果您不喜欢，可以另外做自己的界面。


### 使用方法

理论上，本系统适用于大多数场景使用，特别适合那些人员比较多的单位、工业园区、科技园、学校、村落等等场景使用。

我们知道，一个标准的工业园区或科技园，内部可能有许多不同的小工厂或者企业，人员和访客都比较复杂，为疫情防控增加了困难。

下面就以工业园区为例分别说明一下本系统各个部分的功能和使用方法。

**1、登记系统**

员工（或陌生访客）进入工业园区大门之前，需要核实身份信息，并登记。

分两种情况：工业园区内企业员工和陌生的外来访客。负责园区疫情防控的工作人员，可以事先树立告示牌，提醒不同身份的访客进入不同的检查口排队。

***（1）园区内的企业员工***

针对园区内的企业员工，为了使得整个核验过程简便快捷，避免造成拥挤和排队过久的情况，把核验方式简化为：出示二维码。员工事先通过手机使用自己的账号和密码来登陆疫情防控系统的客户端，登陆成功后，点击对应的“我的二维码”功能，即可由系统自动生成一个带有唯一标识的二维码。负责园区核验的工作人员，则事先使用手机浏览器以“工作人员”身份的账户登陆系统，此时工作人员登陆验证通过的Cookie已自动保存在手机浏览器端，之后直接使用此手机浏览器自带的扫码功能扫一下员工（或访客）展示的二维码，即可自动打开扫码结果页。如果二维码信息验证真实，则会提示核验成功，并且显示该员工的完整信息，如所在公司，姓名，住址，身份证号等等。核验人员通过这些信息比对员工现场出示的身份证信息，确认是否一致。如果信息一致，则给其测量体温，体温正常则允许其进入园区，发热则拒绝进入园区并由园区保安做后续处理。如果信息不一致，要求其按外来访客登记。

***（2）陌生的外来访客***

针对陌生的外来访客，园区核验工作人员可以展示系统注册页面的二维码，访客使用手机扫描此二维码后，会打开一个访客注册/登记的表单页面。访客填写表单并提交完成后，会自动生成一个访客的账号。访客凭账户和密码登陆进入系统后，点击“我的二维码”，展示二维码给工作人员。工作人员扫码后会自动显示该访客填写的信息，根据访客现场出示的身份证和系统显示的信息判断是否填写真实，真实则直接给其测量体温，体温正常则允许其进入园区，发热则拒绝进入园区并由园区保安做后续处理。

**2、疫情上报系统**

疫情上报，是为了加强疫情的管控，园区内的企业员工每天上报自己的体温、是否咳嗽。这样，可以实时准确地掌握园区内所有员工的健康情况，以便及时发现疫情。

**3、疫情公告系统**

在员工登陆的首页的显要位置上，设计了一个专门的新闻公告。可以通过此新闻公告，发布一些疫情相关的信息，比如疫情通知，预防感染知识，等等。通过信息主动告示和宣传，增强员工自身的防范意识，减少新冠肺炎的传播风险。

 
### 更新说明

此源码为1.0.0版本。

### 赞助支持：

支持本程序，请到Gitee和GitHub给我们点Star！

Gitee：https://gitee.com/dengzhenhua/caozha-cepcs

GitHub：https://github.com/dengcao/caozha-cepcs

### 关于

开发：[邓草博客 blog.5300.cn](http://blog.5300.cn)

赞助：[品络互联 www.pinluo.com](http://www.pinluo.com)  &ensp;  [AI工具箱 5300.cn](http://5300.cn)  &ensp;  [汉语言文学网 hyywx.com](http://hyywx.com)  &ensp;  [雄马 xiongma.cn](http://xiongma.cn) &ensp;  [优惠券 tm.gs](http://tm.gs)


### 后端界面预览
![输入图片说明](https://images.gitee.com/uploads/images/2020/0525/172559_95aae385_7397417.png "1.png")
![输入图片说明](https://images.gitee.com/uploads/images/2020/0525/172608_f182105c_7397417.png "2.png")
![输入图片说明](https://images.gitee.com/uploads/images/2020/0525/172615_683685ae_7397417.png "3.png")
![输入图片说明](https://images.gitee.com/uploads/images/2020/0525/173140_340a0811_7397417.png "13.png")
![输入图片说明](https://images.gitee.com/uploads/images/2020/0525/173150_c9c7cb57_7397417.png "14.png")
![输入图片说明](https://images.gitee.com/uploads/images/2020/0525/173157_41132cb9_7397417.png "15.png")
![输入图片说明](https://images.gitee.com/uploads/images/2020/0525/173205_59660680_7397417.png "16.png")
![输入图片说明](https://images.gitee.com/uploads/images/2020/0525/173214_9dbd6a9d_7397417.png "17.png")
![输入图片说明](https://images.gitee.com/uploads/images/2020/0525/173222_4806cd33_7397417.png "18.png")


### 前端界面预览
![输入图片说明](https://images.gitee.com/uploads/images/2020/0525/172653_ce6a8350_7397417.png "4.png")
![输入图片说明](https://images.gitee.com/uploads/images/2020/0525/172701_7f9c43f1_7397417.png "5.png")
![输入图片说明](https://images.gitee.com/uploads/images/2020/0525/172708_07946068_7397417.png "6.png")
![输入图片说明](https://images.gitee.com/uploads/images/2020/0525/172716_213ffdab_7397417.png "7.png")
![输入图片说明](https://images.gitee.com/uploads/images/2020/0525/172724_6e1c9501_7397417.png "8.png")
![输入图片说明](https://images.gitee.com/uploads/images/2020/0525/172733_49f5addd_7397417.png "9.png")
![输入图片说明](https://images.gitee.com/uploads/images/2020/0525/172741_a080b317_7397417.png "10.png")
![输入图片说明](https://images.gitee.com/uploads/images/2020/0525/172805_d3151179_7397417.png "11.png")
![输入图片说明](https://images.gitee.com/uploads/images/2020/0525/172814_f14e5873_7397417.png "12.png")


