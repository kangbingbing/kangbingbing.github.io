---
title: "在线预览PDF及office文件"
layout: post
date: 2017-07-28
image: /assets/images/markdown.jpg
headerImage: false
tag:
- iOS
category: iOS
---


#### 需求：线上生成电子合同，客户端预览。

![](https://ws1.sinaimg.cn/large/9e1008a3ly1fhziu4tsang20ac0ige0o.gif)


这里提供三种方法，不仅限于pdf，经测试，office格式文件同样支持。第二三种方法只能浏览本地文件，所以线上的必须先下载在浏览。

###	第一种方法最简单方便，webView加载。不仅能加载本地，也能加载线上的。

代码如下:

	UIWebView *webView = [[UIWebView alloc] initWithFrame:self.view.bounds];
    webView.backgroundColor = [UIColor whiteColor];
    NSURL *filePath = [NSURL URLWithString:[[NSBundle mainBundle] pathForResource:@"pptdemo" ofType:@"pptx"]];
    NSURLRequest *request = [NSURLRequest requestWithURL: filePath];
    [webView loadRequest:request];
    [webView setScalesPageToFit:YES];
    [self.view addSubview:webView];
	
	

###	第二种方法:本地的话，就直接加载bundle，返回url。线上的先下载到本地，再进行预览。

	NSString *name = @"demo.pdf";
    // 检查本地是否存在
    if ([self isFileExist:name]) {
        
        NSString *path = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES).lastObject;
        NSString *pathString = [path stringByAppendingFormat:@"/%@",name];
        NSLog(@"path:%@", pathString);
        self.pathString = pathString;
    }else{
        
        //重新下载
        [self loadHttpPdfWithUrl:@"http://192.168.1.25/demo.pdf"];
        
    }
    
    // 方法2
    QLPreviewController *QLPVC = [[QLPreviewController alloc] init];
    self.QLPVC = QLPVC;
    QLPVC.delegate = self;
    QLPVC.dataSource = self;
    [self presentViewController:QLPVC animated:YES completion:nil];


**判断是否已经下载**
	
	- (BOOL)isFileExist:(NSString *)fileName{
    
	    NSString *path = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES).lastObject;
	
	    NSString *filePath = [path stringByAppendingPathComponent:fileName];
	    
	    NSFileManager *fileManager = [NSFileManager defaultManager];
	    BOOL result = [fileManager fileExistsAtPath:filePath];
	    
	    NSLog(@"这个文件是否存在%d",result);
	    return result;
	}

**下载的操作**

	- (void)loadHttpPdfWithUrl:(NSString *)url{
    
	    NSURLSessionConfiguration *configuration = [NSURLSessionConfiguration defaultSessionConfiguration];
	    AFURLSessionManager *manager = [[AFURLSessionManager alloc] initWithSessionConfiguration:configuration];
	    //请求
	    NSURLRequest *request = [NSURLRequest requestWithURL:[NSURL URLWithString:url]];
	    
	    //下载Task操作
	    _downloadTask = [manager downloadTaskWithRequest:request progress:^(NSProgress * _Nonnull downloadProgress) {
	        
	        // totalUnitCount;     需要下载文件的总大小
	        // completedUnitCount; 当前已经下载的大小
	        NSLog(@"%f",1.0 * downloadProgress.completedUnitCount / downloadProgress.totalUnitCount);
	
	        
	    } destination:^NSURL * _Nonnull(NSURL * _Nonnull targetPath, NSURLResponse * _Nonnull response) {
	        //返回的这个URL就是文件的位置的路径
	        NSString *documentPath = [NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES) lastObject];
	        NSString *path = [documentPath stringByAppendingPathComponent:response.suggestedFilename];
	        NSLog(@"%@",path);
	        return [NSURL fileURLWithPath:path];
	        
	    } completionHandler:^(NSURLResponse * _Nonnull response, NSURL * _Nullable filePath, NSError * _Nullable error) {
	        //  下载完成
	        NSString *localFilePath = [filePath path];// 将NSURL转成NSString
	        self.pathString = localFilePath;
	        NSLog(@"已经下载完成的路径%@",localFilePath);
	        // 下载完成刷新, 加载
	        [self.QLPVC reloadData];
	        NSLog(@"%@",error);
	        
	    }];
	    // 开始下载
	    [_downloadTask resume];

	}

	
	
**实现代理方法**


	- (NSInteger)numberOfPreviewItemsInPreviewController:(QLPreviewController *)controller{
	    return self.pathString == nil ? 0 : 1;
	}
	- (id<QLPreviewItem>)previewController:(QLPreviewController *)controller previewItemAtIndex:(NSInteger)index{
	    // 加载本地
	    //    NSString *path = [[NSBundle mainBundle] pathForResource:@"pptdemo" ofType:@"pptx"];
	    //    return [NSURL fileURLWithPath:path];
	    
	    // 加载网络下载的, 其实也在本地沙盒了
	    return [NSURL fileURLWithPath:self.pathString];;
	}


###	第三种本地有就直接创建，实现代理方法。线上的下载完成以后创建控制器。实现与方法二类似。

	NSString *path = [[NSBundle mainBundle] pathForResource:@"pptdemo" ofType:@"pptx"];
    UIDocumentInteractionController *docVC = [UIDocumentInteractionController interactionControllerWithURL:[NSURL fileURLWithPath:path]];
    docVC.delegate = self;
    [docVC presentPreviewAnimated:YES];
    
    
**实现代理**

	#pragma mark 方法3代理
	#pragma mark - UIDocumentInteractionControllerDelegate
	- (UIViewController *)documentInteractionControllerViewControllerForPreview:(UIDocumentInteractionController *)controller{
	    
	    return self;
	}
	
	- (UIView*)documentInteractionControllerViewForPreview:(UIDocumentInteractionController*)controller {
	    
	    return self.view;
	}
	
	- (CGRect)documentInteractionControllerRectForPreview:(UIDocumentInteractionController*)controller {
	    return CGRectMake(0, 30, [UIScreen mainScreen].bounds.size.width, [UIScreen mainScreen].bounds.size.height);
	}


**这里下载用的AFN，为了模拟测试，把文件放到本机apache了(mac自带)。电脑本机ip就是服务器地址。路径/Library/WebServer/Documents下。**


代码已上传至 [github](https://github.com/kangbingbing/KBPreviewPDF)

