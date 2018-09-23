---
title: python中编码的那些事
date: 2018-05-08 02:54:13
tags: [Python,编码]
categories: 初级
---
##### TODO 各种编码的区别

- 在Python中,编码分为文本编码,url编码和解释器编码
- 下面从这三个方面来对比Python2 与 Python3的区别

Python2中:
- Python2解释器默认编码为ASCII编码,使用python2时基本都用'utf-8'所以经常会出现下面这种错误,这是由于解释器编码问题导致
>UnicodeDecodeError: 'ascii' codec can't decode byte 0xe4 in position 0

- 解决方案:
 ```python
 # coding=utf-8
 import sys
   reload(sys)
   sys.setdefaultencoding('utf-8')  
 ```

- python2中文本编码,python2中字符串类型一种 str(非unicode) ,一种 unicode,不同编码格式字符串之间不能直接转码,需要先解码(encode('utf8'))到中间状态unicode,再decode('gbk')到想要的编码格式

- python2中url编码与python3差别不大,这个主要是依赖urllib库

- python3中编码问题就简单很多了, 因为python3解释器默认为'utf-8',基本没有解释器编码的问题烦恼

- python3中文本编码也简单许多, 因为python3中的字符串一种str(默认为unicode类型), 一种为bytes类型,str由于默认为unicode,所以不同编码格式之间可以直接转码不需要切换到中间状态
