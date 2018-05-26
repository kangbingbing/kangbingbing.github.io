---
title: "AppStore Review"
layout: post
date: 2017-05-23
image: /assets/images/markdown.jpg
headerImage: false
tag:
- iOS
category: iOS
---



## 前言
最近项目要更新版本，然而以为会跟往常一样，提交后2-3天通过，结果这次可没有没有那么顺利了，两个客户端一起提交上去，相继悲剧。


### 患者端
并且患者端是一个没有见过的原因 PLA1.2, 邮件原文如下。


````
发件人 Apple
PLA 1.2


The seller and company names associated with your app do not reflect the medical institution in the app or its metadata, as required by section 1.2 of the Apple Developer Program License Agreement.

Apps with medical consultation features must be publish by the medical institution.

Next Steps

Your app must be published under a seller name and company name that reflects the medical institution. If you have developed these apps on behalf of a client, please advise your client to add you to the development team of their Apple Developer account.

Once created, you cannot change your seller name or company name in iTunes Connect. For assistance with changing your company name or seller name, you will need to contact iTunes Connect through the Contact Us page. Select Getting Started from the first dropdown menu, then select General iTunes Connect Inquiry to contact the appropriate iTunes Connect team.

````

其中有一句然我看到很绝望，说**具有医疗咨询功能的应用必须有医疗机构发布**，PS:我们应用是和医院合作的，这让医院发布感觉不太现实啊。网上检索了一下，都说最近这种情况很多，金融、P2P、电商，医疗是重灾区。有的说是因为 app 和公司发布账号没有直接性联系，上传下商标软著之类的都给过了，有的是上传的营业执照，五花八门，有的被拒了4，5次过了，有的是把个人账号改成公司账号就行了，可是我们本来就是公司账号。

最后没办法，我也尝试着在邮件内回复，说明软件的目的，并且和和医院的合作协议什么的都上传了，然而第二天收到这样的回复。

````

发件人 Apple
Hello,

Thank you for your response.

Regarding the PLA 1.2 issue, since you have developed this app on behalf of a client, please advise your client to add you to the development team of their Apple Developer account and upload the app in that account.

Best regards,

App Store Review

````


这么看并没有什么用啊，还是要求医院账号发布，把我们账号添加到他们团队里，怎么解决？解决方案？尝试重新打包发布，同样的原因再次被拒。上周四我给Review团队回复了邮件，要求他们电话给我沟通，看怎么解决。他们回复3个工作日内回复我。这周二(今天），早上8点57，一个看起来像虚拟电话的号码打进来，接听手说是Review团队，听起来口音英应该不是大陆这面的（但肯定也不是美国的），应该是香港的，她简单给我描述了下我的问题。大概意思是唯一的解决方案就是要求合作医院的账号来发布这个应用， 什么合同，协议，执照，都不行。那我问像春雨，好大夫，平安好医生，这些问诊类应用怎么不受影响。他说的有句话是重点------**他们是平台**，上面有很多家医院的医生，属平台性质。但是我们也是平台啊，这我才有一点点明白，这个版本被拒是因为应用里面频繁提及合作医院的名字，让审核团队就是觉得这个应用就是给他们定制的，没有体现出是一个平台。


### 医生端
同时医生端的应用就有意思了，患者端悲剧的第二天收到医生端被拒的消息，如下：

````
发件人 Apple

    2. 1 Performance: App Completeness

Guideline 2.1 - Performance


We discovered one or more bugs in your app when reviewed on iPhone running iOS 10.3.2 on Wi-Fi connected to an IPv6 network.

Specifically, we could not log in. Your app did not respond to produce any further actions when tapped on the login button.

Please see attached screenshots for details.

Next Steps

To resolve this issue, please run your app on a device while connected to an IPv6 network (all apps must support IPv6) to identify any issues, then revise and resubmit your app for review.

If we misunderstood the intended behavior of your app, please reply to this message in Resolution Center to provide information on how these features were intended to work.

For new apps, uninstall all previous versions of your app from a device, then install and follow the steps to reproduce the issue. For updates, install the new version as an update to the previous version, then follow the steps to reproduce the issue.

Resources

For information about supporting IPv6 Networks, please review Supporting IPv6 DNS64/NAT64 Networks and About Networking.

````

大概就是说应用不支持 IPV6，登陆不进去。这问题比患者端的 PLA1.2好解决啊，立刻开始在本地搭建 IPV6环境，测试后并没有什么问题。检索了一下，我觉得在网不好的情况下，或者审核团队没有成功登陆之类这些问题被拒原因基本都会是 IPV6，其实原因并不是，只是他们不能正常使用你应用。

代码一行没有改，还是之前的哪个 ipa 包，再次提交审核，这次没有在底下回复，而是重新提交审核了，隔了大概两天，收到这样的回复

````
发件人 Apple
Guideline 2.5.4 - Performance - Software Requirements


Your app declares support for VoIP in the UIBackgroundModes key in your Info.plist, but it does not include any Voice over IP services.

Next Steps

To resolve this issue, please revise your app to either add VoIP features or remove the "voip" setting from the UIBackgroundModes key.

We recognize that VoIP can provide "keep alive" functionality that is useful for many app features. However, using VoIP in this manner is not the intended purpose of VoIP.

Request a phone call from App Review

At your request, we can arrange for an Apple Representative to call you within the next three business days to discuss your App Review issue.

To request a call and ensure we have accurate contact information, reply directly to this message with a contact name and direct phone number to reach you.

````

是说我应用里开启了 VoIP 功能，能后台唤醒，但是从程序里并没有找到哪个功能支持 VoIP，其实程序功能这个版本已经实现了。于是就截了两张图，传过去了。今天早上一看，成功了。怪不得有的人都说，发版的时候，录好一段使用视频，传到 Youtube，提交审核的时候给审核人员看，有利于过审，不无道理啊。


患者端已更改上传，最后结果还不明朗。PS：最近做梦有时候都梦到审核团队给我回邮件。。。。。

#####  2017-5-27  --更新---

由于更换账号时间成本太高，于是只能从产品本身解决问题。从一开始，产品定位就是一个平台，但是由于医院合作紧密，导致频繁提及，那就从产品本身体现平台吧，优化修改了首页，以及相关协议，细节，周四改完，周五提交，今天（周六）中午，通过了。。。。。。

最后**端午小长假要来了。**










