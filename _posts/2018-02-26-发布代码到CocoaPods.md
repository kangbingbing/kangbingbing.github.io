---
title: "发布代码到CocoaPods"
layout: post
date: 2018-02-26
image: /assets/images/markdown.jpg
headerImage: false
tag:
- iOS
category: iOS
---

为自己的项目创建 podspec 文件。，首先通过如下命令初始化一个podspec文件：

	pod spec create your_pod_spec_name
	
your_pod_spec_name即KBCycleScrollView

该命令执行之后，CocoaPods 会生成一个名为your_pod_spec_name.podspec的文件，然后我们修改其中的相关内容即可。

具体配置信息如下

		
	Pod::Spec.new do |s|
	
	  s.name         = "KBCycleScrollView"
	  s.version      = "0.0.1"
	  s.summary      = "A short description of KBCycleScrollView."
	
	  s.homepage     = "https://github.com/kangbingbing/KBCycleScrollView"
	
	  s.license      = "MIT"
	
	  s.author             = { "kang" => "493043919@qq.com" }
	  s.platform     = :ios
	  s.platform     = :ios, "7.0"
	
	  s.source       = { :git => "https://github.com/kangbingbing/KBCycleScrollView.git", :tag => "0.0.1" }
	
	  s.source_files  = "KBCycleScrollView", "KBCycleScrollView/KBCycleScrollView/**/*.{h,m}"
	
	  s.framework  = "UIKit"
	
	  s.requires_arc = true
	  
	  s.dependency 'SDWebImage', '>= 4.0.0'
	
	end


注意s.source_files文件路径配置，文件夹结构如下:

![](https://ws1.sinaimg.cn/large/9e1008a3ly1fottgmdktdj20m106tdhi.jpg)

验证spce文件是否可用

	pod spec lint KBCycleScrollView.podspec --use-libraries --allow-warnings
	
![](https://ws1.sinaimg.cn/large/9e1008a3ly1fottiuuki6j20hz09j3zt.jpg)
	
注意项目由于依赖第三方库, 务必加上 **--use-libraries** 否则会验证失败。

注册 CocoaPods 账号, 邮箱会收到一个验证链接

	pod trunk register 493043919@qq.com 'kangbing'
	
![](https://ws1.sinaimg.cn/large/9e1008a3ly1fottlpxvitj20hv010dfu.jpg)

点击邮箱链接完成验证

![](https://ws1.sinaimg.cn/large/9e1008a3ly1fott09thgfj20x30czq4j.jpg)


发布

	pod trunk push


![](https://ws1.sinaimg.cn/large/9e1008a3ly1fotuk3ey38j20hy0aq0u7.jpg)
