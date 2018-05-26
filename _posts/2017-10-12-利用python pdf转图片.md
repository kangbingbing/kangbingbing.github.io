---
title: "利用python pdf转图片"
layout: post
date: 2017-10-12
image: /assets/images/markdown.jpg
headerImage: false
tag:
- Python
category: Python
---


环境配置

安装ImageMagick

	brew install imagemagick

这里有个坑，brew安装都是7.x版本，使用wand时会出错，需要你安装6.x版本。
解决办法：

1.安装6.x版本

	brew install imagemagick@6

2.取消链接7.x版本

	brew unlink imagemagick

Unlinking /usr/local/Cellar/imagemagick/7.0.7-4… 71 symlinks removed

3.强制链接6.x版本

 	brew link imagemagick@6 --force

Linking /usr/local/Cellar/imagemagick@6/6.9.9-15… 75 symlinks created

4.export环境变量
 echo 'export PATH="/usr/local/opt/imagemagick@6/bin:$PATH"' >> ~/.bash_profile


ok，以上解决imagemagick版本问题。


代码如下:

	#!/usr/bin/python
	#-*- coding: utf-8 -*-
	#encoding=utf-8
	
	from wand.image import Image
	from PyPDF2 import PdfFileReader
	import logging
	import os
	import warnings
	import multiprocessing
	import re
	
	
	def logs():
	    # 禁用警告
	    warnings.filterwarnings('ignore')
	    # 设置logging
	    if not os.path.exists('logs'): os.mkdir('logs')
	    log_filename = 'logs/pdf2img.log'
	    logging.basicConfig(filename=log_filename, filemode='a', level=logging.DEBUG,
	                        format='%(asctime)s %(filename)s [%(levelname)s] %(message)s',
	                        datefmt='[%Y-%m-%d %H:%M:%S]',
	                        )
	
	
	def process(single_page):
	    pattern = re.compile('\[|\]')
	    # 页码
	    page_num = pattern.split(single_page)[1]
	    with Image(filename=single_page, resolution=200) as converted:
	        converted.compression_quality = 45
	        newfilename = file_text + page_num + '.jpg'
	        converted.save(filename=newfilename)
	        logging.info(multiprocessing.current_process().name + '' + newfilename)
	    if not os.path.isfile(newfilename): logging.warning('page of file is not found:[%s]' % (newfilename))
	
	
	def get_pages(filename):
	    return PdfFileReader(file(filename, 'rb')).getNumPages()
	
	
	if __name__ == '__main__':
	    logs()
	    try:
	        get_source = '设计规范模板.pdf'
	        file_text = os.path.splitext(get_source)[0]
	        pages = get_pages(get_source)
	        process_num = multiprocessing.cpu_count()
	        pool = multiprocessing.Pool(processes=process_num)
	        page_list = [get_source + "[" + str(i) + "]" for i in range(pages)]
	        logging.info('=========>>>> Start [Process num: %s]' % (process_num))
	        pool.map(process, page_list)
	        pool.close()
	        pool.join()
	        print 'Page Size:%s' % (pages)
	        logging.info('<<<========= End')
	    except Exception, e:
	        logging.error(e)
	        print 'Page Size:0'


运行

	python pdf2img.py
	
	
![](https://ws1.sinaimg.cn/large/9e1008a3ly1fkffu7jx45j20br086q3u.jpg)
