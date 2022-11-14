# XD安全 学习笔记 | 系统BUG发现

[mp.weixin.qq.com](http://mp.weixin.qq.com/s?__biz=Mzg5MDY2MTUyMA==&mid=2247485087&idx=1&sn=fdfcbb41a884bddbc0fa7698c39f5706&chksm=cfd87960f8aff076358b14b1f3924fd927d8f356fde4114c070ea434783168a966f490766e7f&mpshare=1&scene=1&srcid=0804q4Bm4Lqwo9QdbzC08iHc&sharer_sharetime=1659544853159&sharer_shareid=c75c93b84e50ea6bcab5077411998942#rd)木尤 0x00实验室

声明

作者：团队成员-木尤【无名安全团队】 如需转载本实验室的文章，请标明来源即可。文章仅学习安全技术使用，切记请勿做它用，产生的后果与本公众号无关。

本文是Day42-45天的学习笔记。loudong发现阶段-

* * *

相关名词解释

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZhqdmwSibic1PYMwibQV2q5XmVDGOGIEYG1KNNuOTSCWH6QM4AEH1XHtKA%2F640%3Fwx_fmt%3Dpng)

一、工具的使用

1.Goby下载

    goby下载：https://cn.gobies.org/

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZ7lFv3iaPgEXMBW857wyzpX995HOjn4LbE1eq4dibgteNOMv8zEr7SZMw%2F640%3Fwx_fmt%3Dpng)

下载之后是一个压缩包，解压后目录下有一个Goby.exe文件。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZyiahU47Picj77pZmCichfjRG1vicPF0LghCfLwWjzMjqhr2VtnfF4XSvQA%2F640%3Fwx_fmt%3Dpng)

下载完成后，打开goby是如下界面：

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZyNiadoN4DSIPkDhqNSMgKWroWpvfM1XyLbxp1iamZcKaExLmpVniaT70A%2F640%3Fwx_fmt%3Dpng)

使用

goby的使用比较友好，在点击右上角的+号，输入IP或者域名，选择需要扫描的端口和需要验证的POC即可开始扫描

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZEP3XxuWm1DgLqxTE2ib22a41vkicAOL8oibFb2slRG22jdkymuzDuJbwQ%2F640%3Fwx_fmt%3Dpng)

插件

goby内置了许多的插件，在goby的插件中心可以下载这边插件并使用，下载的时候需要挂代理，不然可能无法加载出来。

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZVgeIA0dAqBxJA6Eqia0uHJUXzSF7QVew9A7gDgGric71vs5GH6YaCo7A%2F640%3Fwx_fmt%3Dpng)

POC

goby支持导入poc，在goby程序目录下的\\golib\\exploits\\user，可以将自己定义的后缀为.json的文件导入此文件夹

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZ41rXPqwAPRceibQU5BwFew7q1X5ZYtWhQxN7iakIA1hqYJIKAowd3uNA%2F640%3Fwx_fmt%3Dpng)

然后在goby的poc管理里面就可以看到刚刚导入的poc

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZ9cknTLBzPsz075scm75teegoU55aTKdcXjkDoulq3fD3hfLyj3qdTw%2F640%3Fwx_fmt%3Dpng)

2.Nessus使用

下载地址：https://pan.baidu.com/s/17uA2OmJbV\_cDG2C6QnHqqA提取码: cxd4

选择进阶扫描

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZRYrgPcx29ch7QMbmcXk2S4Y03eiaiaQ5eY1mG0GHJWeBU1uOqEAZ6BpQ%2F640%3Fwx_fmt%3Dpng)

输入扫描的描述目标等信息

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZxud6aibmbBp9xwR03ze73EnbczK64OlC0o7p8Im1nVJvgNn46GgcScg%2F640%3Fwx_fmt%3Dpng)

选择启动扫描

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZPvqNelcmYLhYaKWgicp8rgXuradokzMMYKGjUuib1cWODWW4pzE5icobg%2F640%3Fwx_fmt%3Dpng)

3.Nmap漏洞扫描

插件库下载：插件库目录：https://github.com/scipag/vulscan

博客：https://www.cnblogs.com/shwang/p/12623669.html

插件库目录

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZkKia4EGbFLwM5lbrthic7MADGzNW7n3qs0FIibdOiaEibrkEyrJicDxz0Opg%2F640%3Fwx_fmt%3Dpng)

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZnaBEuM1uTIqWtDs9W8YZWAwicEWVIkpUyMusiapYpe6ibO4EIJAyM2yfw%2F640%3Fwx_fmt%3Dpng)

二、利用

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZMvialsiaFGhkvsqmdfxMcW6rxRyQnpRAmoUGtKEPVHrZvzunNIN2pKqQ%2F640%3Fwx_fmt%3Dpng)

1.Searchsploit使用

搜索bug下载链接：https://github.com/offensive-security/exploitdb

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZBzHsWibZawvWh7eic5ZiaP41Z4ZAZiaFvhsiblWlP8RZyGbmZag6O515Wog%2F640%3Fwx_fmt%3Dpng)

2.Metasploit使用

输入msfconsole进入界面

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZJN1kibVsJcCicuQvecFVXgrCaLGTt1h88jILruiaJkr4WrSbvymOLItrg%2F640%3Fwx_fmt%3Dpng)

ms 12-020

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZsb5rhPAa9V53LYVU9La996mH10cNP7rHgO79aicFY9k4icexWJov9Kiaw%2F640%3Fwx_fmt%3Dpng)

use到漏洞的name

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZCwWalfpYRmPiasvuXXBw1w1niaia0kxVt7dR1vLUVXQO85Zh2G5E7wCJg%2F640%3Fwx_fmt%3Dpng)

show options显示配置详情

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZjQefuvLNHLavcDNOM12CqqibMXLYiaaWEWP0rf1eibaFnOP9a6aZe6fmA%2F640%3Fwx_fmt%3Dpng)

set rhost 需要攻击的主机IP，设置目标ip

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZAD86t7XZ9LopEfZ8NgLic8xC4UCht71fZXIFMf1VBia5JRfetibkSO6Yg%2F640%3Fwx_fmt%3Dpng)

最后输入exploit开始执行

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZQozRCzRTZnUpACfmf7tIDicVia3Klicp8LmWNthKicn3ehqv9piaibvk8Xkg%2F640%3Fwx_fmt%3Dpng)

最后执行效果为

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZWsvXkSSjScusLWPNEqBb63O5Oabps6icz3QEQO5VRLpz5x3QJajeb6Q%2F640%3Fwx_fmt%3Dpng)

CVE-201-0708 远程代码执行

search 0708

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZE5JUyMMiawwo1FGniaVPaNNzb0pkT6DdU3QlhlicfiahIpSyqiarVB94Wog%2F640%3Fwx_fmt%3Dpng)

user这个漏洞名字，利用的是上述第3个漏洞

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZtaNYVesRStNwx9Adyd52puiaSh61TENEia4fial456N6ef3pdPIw30FHg%2F640%3Fwx_fmt%3Dpng)

show options显示配置详情

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZqyjYwHfYABjibh51icBKKWNojxXLxuibmXibcGbcBsCOmHMM8clGHkOWTg%2F640%3Fwx_fmt%3Dpng)

上图的lhost为反弹的地址，这里需要设置set rhost 目标IP，set lhost 反弹地址（本机地址），lport 反弹的端口输入run后发现攻击失败，输入show targets显示利用的系统版本信息

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZnq3HRQ3ib1kn6XPsQicoW7S9WofgoGve59bkzBSxhs3PjqXiaeuB14NBQ%2F640%3Fwx_fmt%3Dpng)

set target 2 ，设置目标系统版本信息为第二个，然后run攻击成功

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZYibggdgdr0C5iaNf4LkpU70CTOt39OLvVtquIObvmiaaKeKhxicpTp7rlQ%2F640%3Fwx_fmt%3Dpng)

web应用bug发现

一、已知CMS

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZHm5Z0zaNYqvaaibeuYyDJ1Pj8mtAanE2vVtcJ75XRODN4jNOL0xicTKQ%2F640%3Fwx_fmt%3Dpng)

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZLhqzRKqZChBEbicTML6OOns8p3KrBLeWoOty8IrrG6EDafLgib7T1Zxg%2F640%3Fwx_fmt%3Dpng)

二、开发框架

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZNMZRF388dwxkUZGia8XbFbibpCO6s3pWWaCSEa3uWlccuDeiacZwKVmQg%2F640%3Fwx_fmt%3Dpng)

ThinkPHP框架

参考链接：https://github.com/SkyBlueEternal/thinkphp-RCE-POC-Collection

Sring框架

参考链接：https://vulhub.org/#/environments/spring/CVE-2018-1273/

三、未知CMS

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZvqRZBNMyAvhGvNRaAZTzLOt4h8o3scSevDeC1IpicUOvG5omia55qIPg%2F640%3Fwx_fmt%3Dpng)

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZianQBPyCB4c7R2uF4vDOBEycHiaRQK5kRIJo4zfnicoZfTvN5boib2dSCA%2F640%3Fwx_fmt%3Dpng)

APP应用bug发现

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZRricwQz2LeN2CWz1gUSleJfdJichB1rGsctUD4nx40YXPbuOcLNtB4cw%2F640%3Fwx_fmt%3Dpng)

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZ9qXBGoGpljU1xiaEE1OmasBWGpQ5L8pn3azuo1U3BkfylIlOjWfUGIw%2F640%3Fwx_fmt%3Dpng)

抓包

1.在电脑上安装一个模拟器

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZ3Uzibce4m2TAibqf5uoBQ9Nsdictm85aiaOh1Bqxfw36icI1bTZHJz4unSA%2F640%3Fwx_fmt%3Dpng)

2.在模拟器的设置里面修改网络配置，设置HTTP代理，IP地址为本机电脑的IP地址

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZPzRqic8r7rICnOGkopQVfq2SfL2rYHytVD65wvzNMlDqrDFQu50zABw%2F640%3Fwx_fmt%3Dpng)

3.在burpsuite的代理模块配置当前虚拟机设置的代理地址和端口，也就是本机的IP，和上图定义的端口

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZjQJxlUJCBBAN9AupPYY2ibTHttN5ejQ3YvtoyYurLZdibAfKWYyXiceDA%2F640%3Fwx_fmt%3Dpng)

4.这个时候在模拟器打开的APP流量都会经过burp，burp开启抓包功能即可抓到数据包

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZmZDzBNg78w3ia8VcWs3jRjQOfClvlGn9rnNFuyicZqGKRuCMMEcpZADg%2F640%3Fwx_fmt%3Dpng)

逆向

1.apk数据提取，将apk安装包放到这个apps目录下面

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZxvFNaTf32cIJH03R2Fp6hvu6uOKx2FI9dOb5yuUdEbf9GZzxicYH26A%2F640%3Fwx_fmt%3Dpng)

2.当apk文件放进去之后运行apkAnalyser.exe

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZsSL6znL3U13ibQ1PumWqrVT3QcgCLnDBP4Y2Oo5A1YroCatV893lT8A%2F640%3Fwx_fmt%3Dpng)

3.当提取完成后，结果会放在result文件夹下面

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZzjPhV59EWRiceShRUZZ0yP8OR0YnqicibF4HoGpz1ictGZrIOBGuuKdGLw%2F640%3Fwx_fmt%3Dpng)

xray的使用

1.先用powershell运行文件目录下的exe文件夹，需要传递监听地址和端口

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZOicWJqN3V7CE62EsG3xVFDOjiaAhqbPQT8cQ5Wt3WZ2k5QgxZcdvTQIA%2F640%3Fwx_fmt%3Dpng)

2.在burp这个位置设置上图监听的端口

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZskOChxNOcbdzLAz22icFhIrmg3NRYlDuCyocxXOYmIndlAFNvRqLWMg%2F640%3Fwx_fmt%3Dpng)

3.设置模拟器的网络连接为当前本机的ip

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZhG7pibhl06z1rjDreDFnBia9C1dYqMFIybXujHPlwobIOVR0lJOmiahibQ%2F640%3Fwx_fmt%3Dpng)

4.成功抓到数据包

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZSZ3F7C4jOpia26hqd1X7XxedOqyF7mf2Tgj0Uia6HMXn9oyzFH3X6TYw%2F640%3Fwx_fmt%3Dpng)

API服务漏洞发现

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZBA3uemZwChN9ava5pVhT3RpwUMwkdttWCHPynJ0DxC4BOha83MWVvw%2F640%3Fwx_fmt%3Dpng)

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZGuywKfM5zTuNfVyur4BmhN6y7y2dOf3d9zcsuUlYOlQ2zlku6XLZ4g%2F640%3Fwx_fmt%3Dpng)

1.tomcat默认配置页弱口令上传war包

找到tomcat默认页

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZ2kTREL6gMlsibo4nXibzRpOcY6lftR8wgStgzGIqPOukP7fCqPjCGk2A%2F640%3Fwx_fmt%3Dpng)

输入账号密码

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZOq7xzwhrkrZeP2jKuI2etev4uBAXc0IGibBpOI71zia7gz0ricMedzR3w%2F640%3Fwx_fmt%3Dpng)

进入后台

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZPxERK0ibAB3qBRUAuM1yCRtc0q4Wh8hcXtTmhBmcbhXnUmaOvRMkHZw%2F640%3Fwx_fmt%3Dpng)

2.超级弱口令检测工具

![](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_png%2FcbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZxbYSiaevWjCBUyjYoOhs1GqwOVKyKy7iaibwibnzEmtYznT9Ou9YicCc48Q%2F640%3Fwx_fmt%3Dpng)

    总结：

[查看原网页: mp.weixin.qq.com](http://mp.weixin.qq.com/s?__biz=Mzg5MDY2MTUyMA==&mid=2247485087&idx=1&sn=fdfcbb41a884bddbc0fa7698c39f5706&chksm=cfd87960f8aff076358b14b1f3924fd927d8f356fde4114c070ea434783168a966f490766e7f&mpshare=1&scene=1&srcid=0804q4Bm4Lqwo9QdbzC08iHc&sharer_sharetime=1659544853159&sharer_shareid=c75c93b84e50ea6bcab5077411998942#rd)