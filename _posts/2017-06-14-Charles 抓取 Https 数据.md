---
title: "Charles 抓取 Https 数据"
layout: post
date: 2017-06-14
image: /assets/images/markdown.jpg
headerImage: false
tag:
- iOS
category: iOS
---


### 下载
首先去下载 [Charles](https://www.waitsun.com/?s=Charles)，安装之后，![](http://ww1.sinaimg.cn/large/9e1008a3ly1fgksjxatz2j20ij0flq4y.jpg)  

### 设置
查看本机 IP, 然后手机连接到同一个 WiFi，并且手动设置代理，服务器为刚才看的 IP，端口8888，如图![](http://ww1.sinaimg.cn/large/9e1008a3ly1fgksl64k6mj20ku112grq.jpg) 

点击返回。连接成功后打开 Charles 就可以看到请求的数据了。如图：![](http://ww1.sinaimg.cn/large/9e1008a3ly1fgksng42jij20op0j640w.jpg)

但是如果是 https 就肯定就不行了，![](http://ww1.sinaimg.cn/large/9e1008a3ly1fgksq10r2cj20op0jdn2n.jpg)

### Https
那就安装证书进行如图操作
![](http://ww1.sinaimg.cn/large/9e1008a3ly1fgkstifzzcj20r30aw0yg.jpg)

点击后出现弹框
![](http://ww1.sinaimg.cn/large/9e1008a3ly1fgksswvtnrj20ln04ewet.jpg)
这时候打开手机 Safari， 地址栏输入 chls.pro/ssl, 安装描述文件就可以了，这个我已经安装过了。
![](http://ww1.sinaimg.cn/large/9e1008a3ly1fgksvnvsvtj20ku112q6m.jpg)

这是再次打开发现还是获取不到https数据，来进行下一步操作。
![](http://ww1.sinaimg.cn/large/9e1008a3ly1fgkt738eekj20jf0d3teh.jpg)

按照下图操作。
![](http://ww1.sinaimg.cn/large/9e1008a3ly1fgkt6dh2czj20oi0k0aeh.jpg)

点击添加，把知乎的 URL 填进去，端口443；点击 OK， 确定
![](http://ww1.sinaimg.cn/large/9e1008a3ly1fgkt6oysayj20d0068aaq.jpg)

这时候已经大功告成啦。
![](http://ww1.sinaimg.cn/large/9e1008a3ly1fgkt6unbzcj20o50cq40q.jpg)

有问题随时留言。 







