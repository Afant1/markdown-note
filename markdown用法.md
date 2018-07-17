# markdown用法
### 根据标题生成目录 `[TOC]`
-----
[TOC]

#快捷键
 
###ctrl+k 代码区域

    print "test"
### ctrl+2 二级标题
### ctrl+b/i 粗体/斜体
**test** *test*
###ctrl+l 插入链接
[百度链接](http://baidu.com)
### ctrl+g 插入图片
![Alt text](./Nmap 思维导图.png)
### ctrl+alt+t 建立表格
:-----------左对齐
:-----------:居中
-------------:右对齐
| 表1          |     表2   |   表3    |
| :----------- | --------:| :------: |
| field1       |   field2 |  field3  |
| field1       |   field2 |  field3  |

### ctrl+/ 马克飞象帮助文档
# 具体语法
### ~~删线~~:~~删线的文字~~
###Ctrl+r分隔符:- - - - - - - - - -

-------------------------------------
### 单行代码 ``
`print "hello"`
### 指定代码:` ```javascript code``` `
```python
import optparse
import sys


def print():
	print("hello world")
```
**javascript**
```javascript
function print()
{
	console.log("hello");
}
```
### 无序列表 ` -(空格)`
- **列表1** : 
- **列表2** :
### 有序表 `1.(空格)`
1. test1
2. test2
3. test3
>可多层嵌套
1. one
	 1. a
	 2. b
2. two
	- a 
	- b
### 注释`>`
>**注意: **这是一个注释。
### 邮箱`<afanti@ncepu.edul.cn>`
<afanti@ncepu.edul.cn>
### 设置字体、字号、颜色

```
<font face="黑体">我是黑体字</font>
<font face="微软雅黑">我是微软雅黑</font>
<font face="STCAIYUN">我是华文彩云</font>
<font color=#0099ff size=12 face="黑体">黑体</font>
<font color=#00ffff size=3>null</font>
<font color=gray size=5>gray</font>
```

<font face="黑体">我是黑体字</font>
<font face="微软雅黑">我是微软雅黑</font>
<font face="STCAIYUN">我是华文彩云</font>
<font color=#0099ff size=12 face="黑体">黑体</font>
<font color=#00ffff size=3>null</font>
<font color=gray size=5>gray</font>
### 注释`<!-- 这是注释-->`
<!-- 这是注释-->