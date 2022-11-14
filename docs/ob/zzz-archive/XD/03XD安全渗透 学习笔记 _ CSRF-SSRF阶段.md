# XD安全渗透 学习笔记 | CSRF-SSRF阶段

[mp.weixin.qq.com](http://mp.weixin.qq.com/s?__biz=Mzg5MDY2MTUyMA==&mid=2247484353&idx=1&sn=41ebcb2d6503d0a1cbf8ba59938db15f&chksm=cfd87c3ef8aff5285c0c6614a4be30c9f944d02ad32cfe150c5c52e6e81660518fa515532d5a&mpshare=1&scene=1&srcid=0804ulNO417vfrdPMmUXFesE&sharer_sharetime=1659544813788&sharer_shareid=c75c93b84e50ea6bcab5077411998942#rd)Lemon 0x00实验室

来源于团队成员Lemon的学习笔记。本系列会定期更新 麻烦各位点个关注。

* * *

往期回顾：

day58-64[XD安全渗透笔记 | 提权阶段](http://mp.weixin.qq.com/s?__biz=Mzg5MDY2MTUyMA==&mid=2247483877&idx=2&sn=35f4bdce1d4043219dbbb67420bb5818&chksm=cfd87e1af8aff70c74e6a5541d7b120548ac3242a320083a81f4b0ae3971d60b2903e7245705&scene=21#wechat_redirect)-

day65-66[XD安全渗透笔记 | 内网渗透(一)](http://mp.weixin.qq.com/s?__biz=Mzg5MDY2MTUyMA==&mid=2247484046&idx=1&sn=85b730dafd28d0e798b8c70fe8130582&chksm=cfd87d71f8aff4672c1c0d254342b6e63f026b825be225e8af7fa30f364cf49bbdb384b42b0f&scene=21#wechat_redirect)

day67-68[XD安全渗透笔记 | 内网渗透(二)](http://mp.weixin.qq.com/s?__biz=Mzg5MDY2MTUyMA==&mid=2247484112&idx=1&sn=036ffcfd03b4a4ee94f6f787cd7986a7&chksm=cfd87d2ff8aff439d9c2a89c2054b471a05c800e7a3d7ee605ef642b3321cab54b1423243454&scene=21#wechat_redirect)

day69-70[XD安全渗透笔记 | 内网渗透(三)](http://mp.weixin.qq.com/s?__biz=Mzg5MDY2MTUyMA==&mid=2247484247&idx=1&sn=abc131479e3df8a5389f97452d5e0241&chksm=cfd87ca8f8aff5be206c7f90aaef732828abafc98d840f90f9b31ba8e123cf3cc8091ea27cd2&scene=21#wechat_redirect)

day71-72[XD安全渗透笔记 | 内网渗透(四)](http://mp.weixin.qq.com/s?__biz=Mzg5MDY2MTUyMA==&mid=2247484318&idx=1&sn=206ece7fccf290a6c6fdcb5b6a999cea&chksm=cfd87c61f8aff577216c821de153e21567488b784d5e1283fedd6a860328166ecc6b37e27259&scene=21#wechat_redirect)

* * *

day29 CSRF和SSRF漏洞案例讲解

CSRF什么是CSRF ？怎么做？

**-
**CSRF:跨站请求伪造（Cross-site request forgery）CSRF是一种挟制用户在当前已登录的Web应用程序上执行非本意的操作的攻击方法。当用户访问含有恶意代码的网页时,会向指定正常网站发送非本人意愿的数据请求包（如转账给hack,向hack发送API等）如果此时用户恰好登录了该正常网站（也就是身份验证是正常的）就会执行该恶意代码的请求，从而造成CSRF。

与在XSS章节中提到的在博客里写入获取cookie的代码，在管理员登录后台查看时就会窃取其cookie有异曲同工之妙跟跨网站脚本（XSS）相比，XSS 利用的是用户对指定网站的信任，CSRF 利用的是网站对用户网页浏览器的信任

pikachu靶场试验CSRF模块里登录进去抓个包看看"修改个人信息"的请求包 理论上来说 如果攻击者在自己的页面伪造了含有这个请求包的代码，一旦用户上钩点进恶意网站且pikachu属于登录状态就会触发CSRF漏洞

1）win10：pikachu靶场（模拟正常登录网站）【IP：192.168.168.1】

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ81gsAzy2M1YibTsuPsKhU3ZrdXTJF2SumINxhiafHp5DR4G7PfxDpvFgxhCJF5UDAMVlNmCNSp7VAw%2F640%3Fwx_fmt%3Dpng)

抓包查看请求数据包-

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ81gsAzy2M1YibTsuPsKhU3ZIDeu7WYcXNud8UMugE4nibm6uibOb2pwyhYhOa2enRIH0KT4SJo2Nl2A%2F640%3Fwx_fmt%3Dpng)

这里是以GET方式请求 并将需要修改的数据传给服务器-

ps：这里出现了Cookie，如果利用XSS漏洞在页面上插入并将Cookie发送给攻击者，那么攻击者会有机会直接登录用户的账号

2）win7：攻击者的恶意网站 【IP：192.168.168.128】

index.html

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ81gsAzy2M1YibTsuPsKhU3ZUjvqk5EbPmK2F9bHb7vSb25cNCzPHicTnMpuMqFUomsVo7ubhtQ24MQ%2F640%3Fwx_fmt%3Dpng)

3）用户访问攻击者恶意网站且pikachu是登录状态

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ81gsAzy2M1YibTsuPsKhU3ZEicw6SuZ3Lbnt8oQPSf3WibM8ec9S2ujwdWXrMAY66GI4om4jP79XhVw%2F640%3Fwx_fmt%3Dpng)

可以看到 用户如果没有查看将会无感的向攻击者指定的网站发送非本人的请求-

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ81gsAzy2M1YibTsuPsKhU3Z3wCAv7kdib6DuUIgBP24djg0mibeWiceicuvT7U2pLb8Bf6sicOCibL5wa1A%2F640%3Fwx_fmt%3Dpng)

且会发现该请求带上了正确的Cookie（对网站来说视为其本人发起了一个请求且通过了Cookie验证）

4）返回页面查看，发现信息已被篡改

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ81gsAzy2M1YibTsuPsKhU3ZhiakQtJdpRgoJ7WDTG1SMxHdhwu0Tiaj1zzb1qQGHicgEOt0JxsIJia41Q%2F640%3Fwx_fmt%3Dpng)

小Tips：

如果遇到了POST型，则需要构造一个表单提交按钮欺骗用户点击PS：超星尔雅学习通同样存在这样的CSRF，手法与该靶场演示手法类似，在校期间和同学进行实验点击链接后就可以更改其性别手机号等（当时戏称一键泰国变性XD）提交后对方以未对敏感数据造成影响打回了目前尚未修复，感兴趣的小伙伴可以自行尝试，手法及其相同，这里就不赘述了。

    #防御方案

* * *

SSRF相关知识

什么是SSRF？

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ81gsAzy2M1YibTsuPsKhU3ZtWHyWiaG8uQibBRf6w0cys7NXadKVViaMWzy7qaumAiaQkFq8uFU8g3Llg%2F640%3Fwx_fmt%3Dpng)

SSRF：服务器端请求伪造（Server-Side Request Forgery）

SSRF是一种由攻击者构造形成由服务端发起请求的一个安全漏洞。SSRF攻击的目标一般是从外网无法访问的内部系统。（正是因为它是由服务端发起的，所以它能够请求到与它相连而与外网隔离的内部系统）

SSRF 形成的原因大都是由于服务端提供了从其他服务器应用获取数据的功能且没有对目标地址做过滤与限制。比如从指定URL地址获取网页文本内容，加载指定地址的图片，下载等等。利用的是服务端的请求伪造。ssrf是利用存在缺陷的web应用作为代理攻击远程和本地的服务器

常见漏洞点与关键字

漏洞挖掘时可以参考这些点入手

WEB功能上：

1）分享：通过URL地址分享网页内容-

2）转码服务：通过URL地址把原地址的网页内容调优使其适合手机屏幕浏览

3）在线翻译：通过URL地址翻译对应文本内容 如：百度，有道

4）图片加载与下载：通过URL地址加载与下载图片

5）图片 文章收藏功能

6）未公开的API实现以及其他调用URL的功能

URL中的关键字：【结合谷歌语法找到入手点】 share wap url link src source target u 3g display sourceURL imageURL domain

实例:自己搭建的测试环境

靶机：win7 （192.168.168.128）攻击机：win10 （192.168.168.1）

1）首先在靶机里部署一个页面模拟一个访问网站的业务（类似于翻译网页的网站 给出一个网站后它会主动去访问并翻译内容返回）再放置一个文件方便看来源

ipindex.php

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ81gsAzy2M1YibTsuPsKhU3Zr1Sz9AwvjmVvfp4Lm7OdjU8fyY9iabEWkicW8aoNyuR57A9Nl9NIagMw%2F640%3Fwx_fmt%3Dpng)

1.php （用于看访问它的IP是多少）

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ81gsAzy2M1YibTsuPsKhU3Z2Pfkft0gcMwCxd9eOaiaHwnZVsKy0nc9n8xlqoS6RaOmcMjAicibarDaA%2F640%3Fwx_fmt%3Dpng)

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ81gsAzy2M1YibTsuPsKhU3Zkue1ZIyvLAjO7YBllqR8krTyVX7QXMDBiaUE09j2IvnYsapPDWepbPw%2F640%3Fwx_fmt%3Dpng)

图示点击后就会跳转到百度这个页面（这里为了方便仅有跳转功能 真实环境的翻译等等没有做）

2）以攻击机身份访问该网站，可以看到直接访问1.php

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ81gsAzy2M1YibTsuPsKhU3Z2WSH5icHwKzr0PoTWjoFUkzv48VMLE5P0Oc9z36L7CoRpH45ibP6ia2Jg%2F640%3Fwx_fmt%3Dpng)

在想翻译的网站这里直接输入 http://192.168.168.128:8089/SSRF/1.php

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ81gsAzy2M1YibTsuPsKhU3Zyq2Sdl94QfR8UPwc7VS4t7td5Oo55WnyB9RTJr6Nv40jWVClD4VQibg%2F640%3Fwx_fmt%3Dpng)

换成http://127.0.0.1:8089/SSRF/1.php 可能会更直接点

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ81gsAzy2M1YibTsuPsKhU3ZpKgZhx5WI8a1EL3w9fbMQ0FXJPURYVpBtzgHUcBdGXIYD7TR9ib8RMQ%2F640%3Fwx_fmt%3Dpng)

可以发现通过这样的方式会让攻击者以内部网络身份访问文件(也就是说这样可以让我们访问一些原本不允许外网访问的文件) 通过SSRF漏洞访问了内网资源

试试访问Mysql 如果来自外部自然是被拒绝的 而如果是内部则会被允许

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ81gsAzy2M1YibTsuPsKhU3ZnxBH1lE4LPJka8RuqibAvn0XlgR40ElibVDNliajmYudzhrwpkGCs9mXw%2F640%3Fwx_fmt%3Dpng)

直接访问数据库接口是不被允许的

在页面输入http://127.0.0.1:3306提交 会发现可以查询到数据

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ81gsAzy2M1YibTsuPsKhU3ZWQqOJNp1ISOKtNHKGsMq8AibyLCia5gyk2R05VSDcEfvJTRVkiapSOZicw%2F640%3Fwx_fmt%3Dpng)

进一步思考 这样可以对内网的端口进行探测 实战中需要用字典跑一跑内网地址

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ81gsAzy2M1YibTsuPsKhU3Z4jrotPfCDDISSIHmUUToXib9d6e1n9TWGHM5SU84lfjEaWaxTjoUTqA%2F640%3Fwx_fmt%3Dpng)

tips

如果以file协议去访问 结合目录穿越漏洞 可以得到敏感信息

如：?url=file:///../../../../../../etc/password

HFS的RCE

    找到的版本只有2.3m 此时已不存在该RCE所以仅写出payload当思路

    https://pan.baidu.com/s/1bp96ECJ    //CSRFTester 

* * *

day30 RCE代码及命令执行漏洞全解

什么是RCE？怎么产生的？

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ81gsAzy2M1YibTsuPsKhU3Zt5aMicIPpoN6bDsD3t7E0cFKuhkNB4WyQc7uibsG4x9v3MRpJ2UC662Q%2F640%3Fwx_fmt%3Dpng)

RCE:远程命令/代码执行(remote command/code execute)

在 Web 应用中有时候程序员为了考虑灵活性、简洁性，会在代码调用代码或命令执行函数去处理。比如当应用在调用一些能将字符串转化成代码的函数时，没有考虑用户是否能控制这个字符串，将造成代码执行漏洞。同样调用系统命令处理，将造成命令执行漏洞。

一般出现这种漏洞，是因为应用系统从设计上需要给用户提供指定的远程命令操作的接口

pikachu靶场试验

第一步：判断操作系统 抓包 在server里查看即可

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ81gsAzy2M1YibTsuPsKhU3ZbmhfesFjkL4kVT7kyPV1ib5rx7iayib5G73DnaI5O9icBY6fHHeyoDicnjg%2F640%3Fwx_fmt%3Dpng)

可以看到部署在windows上 所以RCE执行命令时需要使用windows的命令而不是linux

第二步：该靶场模块是实现一个ping的功能 也就是说输入的东西会被其在cmd中执行可控参数+漏洞函数 构成了出现漏洞的前提条件

在框内输入127.0.0.1 | dir 会发现返回的结果中执行了dir命令-

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ81gsAzy2M1YibTsuPsKhU3ZyXZC0S3bHkQGWM3pNibKhUNnzPE8f7EfGaKjaQ12MAaP6nNzHvwa1Aw%2F640%3Fwx_fmt%3Dpng)

一段代码带来的思考

视频中出现了一段带加密的源码 经过解码后得到语句

    eval(echo `$_REQUEST['a']`);

如果这里出现RCE 是要执行Linux系统命令还是php命令呢？

因为echo的存在 所以直接给a传linux命令就可以了 真实环境中需要具体分析

webmin的RCE

CVE-2019-15107 版本<=1.910 在vulhub进行复现抓包

由于其在修改密码处存在后门植入 可以利用报错信息越权执行命令

    

此处利用其密码错误的报错执行命令处old进行RCE

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ81gsAzy2M1YibTsuPsKhU3ZicZ9vC5346pX1cMuhgSL3QI6plW4Ekj2ibOCdSsvmpwibvT7IZJ9lZCUQ%2F640%3Fwx_fmt%3Dpng)

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ81gsAzy2M1YibTsuPsKhU3ZDAObr0eHYlqk5NcsrwMD1MSAlZ0CVstxbvOJu62jteccLiaFh82eHMg%2F640%3Fwx_fmt%3Dpng)

可以发现在报错信息里返回了攻击者插入的命令的执行结果

小结：这里是利用密码重置功能发生了缺陷，对用户输入未进行过滤，通过管道符实现命令执行，只有在发送的user参数的值不是已知Linux用户的情况下，才会进入到修改 /etc/shadow 的地方，触发命令注入漏洞。

进一步的试验

在vulhub上有大量的有关Struts2各个版本的RCE漏洞 有空了一一复现看看逻辑（去补代码知识了）题外话：菜刀 蚁剑 与 一句话木马 本质上就是RCE

    https://www.cnblogs.com/ermei/p/6689005.html //JAVA web网站代码审计 

[查看原网页: mp.weixin.qq.com](http://mp.weixin.qq.com/s?__biz=Mzg5MDY2MTUyMA==&mid=2247484353&idx=1&sn=41ebcb2d6503d0a1cbf8ba59938db15f&chksm=cfd87c3ef8aff5285c0c6614a4be30c9f944d02ad32cfe150c5c52e6e81660518fa515532d5a&mpshare=1&scene=1&srcid=0804ulNO417vfrdPMmUXFesE&sharer_sharetime=1659544813788&sharer_shareid=c75c93b84e50ea6bcab5077411998942#rd)