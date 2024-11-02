---

title: re
date: 2023-04-20 22:26:25
description: re正则化表达式学习
tags:
- 正则表达式
categories:
- 编程学习
music:
 server: netease   # netease, tencent, kugou, xiami, baidu
 type: song        # song, playlist, album, search, artist
 id: 16846091      # song id / playlist id / album id / search keyword
author: 林宇轩
---

- ## 七个境界

  ```python
  import re 
  # level1:固定字符串
  text = '麦叔身高:178，体重：168，学号：123456，密码:9527'
  # findall()方法的第1个参数是模式，第2个参数是要查找的字符串。
  # re的findall()方法找到所有符合模式的字符串，返回一个列表。
  # r表示这是一个raw字符串，让Python不要去转义里面的特殊字符
  print(re.findall(r'123456', text)) 
  
  # level2 - 某一类字符
  # \d表示所有的数字，1,7,8,1,6,8等都可以匹配到
  print(re.findall(r'\d', text))
  
  # level3 重复某一类字符
  # 增加了+号，表示数字可以出现1到多次，所以178等都符合它的要求。
  print(re.findall(r'\d+', text))
  
  # 组合level4，5 -> 找出座机号
  # \d{4}-\d{8}这是一个组合的模式，表示前面4个数字，中间一个横杠，后面8个数字。
  text = '麦叔电话是18812345678，他还有一个电话号码是18887654321，他爱好的数字是01234567891，他的座机是：0571-52152166'
  print(re.findall(r'\d{4}-\d{8}|1\d{10}', text))
  
  # 组合level6限定位置，7内部约束
  # ^符号，表示一定要在句子的开头才行
  # \w{3}表示3个字符，放在小括号中(\w{3})就成为一个分组，而后面的(\1)表示它里面的内容和第1个括号里的内容必须相同，其中的1就表示第1个括号，也就是说3个字符要重复出现两次。
  print(re.findall(r'^1\d{10}|^\d{4}-\d{8}', text))
  print(re.findall(r'(\w{3})(\1)', text))
  ```

  ## 书写步骤

  - 确定模式包含几个子模式
  - 各个部分的字符分类是什么
  - 各个子模式如何重复
  - 是否有外部位置限制
  - 是否有内部制约关系

  ## 正则表达式Cheatsheet

  ![](https://s2.loli.net/2023/04/20/rvGdeRY81a2kyNJ.png)

  ## re模块方法

  - re.search()：查找符合模式的字符，只返回第一个，返回Match对象
  - re.match()：和search一样，但要求必须从字符串开头匹配
  - re.findall()：返回所有匹配的字符串列表
  - re.finditer()：返回一个迭代器，其中包含所有的匹配，也就是Match对象
  - re.sub()：替换匹配的字符串，返回替换完成的文本
  - re.subn()：替换匹配的字符串，返回替换完成的文本和替换的次数
  - re.split()：用匹配表达式的字符串做分隔符分割原字符串
  - re.compile()：把正则表达式编译成一个对象，方便后面使用
