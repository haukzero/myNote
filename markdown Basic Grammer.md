# Markdown Basic Grammer
## 标题
标题分级用1~6个<mark>`#`</mark>表示不同程度的标题

## 目录

语句：==`[toc]`==或==`[TOC]`==

## 字体、字号、字色
- 斜体: <mark>`_内容_`</mark>或<mark>`*内容*`</mark>
- 粗体: <mark>`__内容__`</mark>或<mark>`**内容**`</mark>
- 粗斜体: <mark>`___内容___`</mark>或<mark>`***内容***`</mark>
- 其他字体: <mark>`<font face='字体'>内容</font>`</mark>
- 字号: <mark>`<font size=数字>内容</font>`</mark>
- 字色: <mark>`<font color=颜色>内容</font>`</mark>
- 字体字号字色的连用: <mark>`<font face= size= color=>内容</font>`</mark>

## 文字居中、居右
### 居中
- <mark>`<center>内容</center>`</mark>
- <mark>`<div align=center>内容</div>`</mark>
- <mark>`<p align="center">内容</p>`</mark>
- <mark>`<h5 style="text-align:center">内容</h5>`</mark>
### 居右
- <mark>`<div align=right>内容</div>`</mark>
- <mark>`<p align="right">内容</p>`</mark>
- <mark>`<h5 style="text-align:right">内容</h5>`</mark>

## 高亮、背景颜色
- 高亮: 
  - <mark>`<mark>内容</mark>`</mark>
  - ==`==内容==`==
- 背景颜色: <mark>`<mark style=background-color:背景颜色>内容</mark>`</mark>

## 下划线、删除线
- 下划线:<mark> `<u>内容</u>`</mark>
- 删除线: <mark>`~~内容~~`</mark>

## 换行
- 直接在一句话后敲两个空格
- 两句话间加一个空行
- 换行处加<mark>`<br/>`</mark>

## 空格、空行
### 空格
- 不换行空格，全称 no-break space：<mark>`&nbsp;`</mark>
- 半角空格，全称 en space：<mark>`&ensp;`</mark>
- 全角空格，全称 em space：<mark>`&emsp;`</mark>
- 细小空格，全称 thin space：<mark>`&thinsp;`</mark>
### 空行
- 空行的标记和不换行空格标记一样，只需单独放一行即可
- `<br/>`

## 分割线
在一行中用三个<mark>`-`</mark>或<mark>`*`</mark>来建立一个分割线
<font color=red>ATTENTION</font>:在分割线上面要空一行！！！

## 引用
- <mark>`>引用内容`</mark>
- 引用分级: 连用几个<mark>`>`</mark>

## 链接
- <mark>`[链接名称](链接地址)`</mark>
- <mark>`<链接地址>`</mark>

## 图片
直接拖拽或复制粘贴图片

## 列表
- 无序列表，用<mark>`*`</mark>、<mark>`+`</mark>、<mark>`-`</mark>再加一个空格
- 有序列表，用数字加上<mark>`.`</mark>号，再加上一个空格
- 控制列表的层级，则需要在列表符号前使用<mark>`Tab`</mark>

## 代码块
- 在一行内引用代码，用反引号<mark>`</mark>引起来
- 在一个块内引用代码，需要用三个反引号引起来，同时在前一个反引号后写入代码的语言
支持的代码语言:
```
bash
c,clojure,cpp,cs,css
dart,dockerfile,diff
erlang
go,gradle,groovy
haskell
java,javascript,json,julia
kotlin
lisp,lua
makefile,markdown,matlab
objectivec
perl,php,python
r,ruby,rust
scala,shell,sql,swift
tex,typescript
verilog,vhdl
xml
yaml
```

## 表格
表格使用 <mark>`|`</mark> 来分割不同的单元格，使用 <mark>`-`</mark> 来分隔表头和其他行
- <mark>`:-`</mark>: 将表头及单元格内容左对齐
- <mark>`-:`</mark>: 将表头及单元格内容右对齐
- <mark>`:-:`</mark>: 将表头及单元格内容居中

## 脚注
- 标号：内容`[^标号]`
- 注释：`[^标号]:注释内容`

<font color=red>ATTENTION</font>：
- 脚注会自动被搬运到最后面，且脚注后面的链接可以直接跳转回加注地方
- 标号可以随意但不能有相同，Markdown会自动调整为顺序的阿拉伯数字

## 特殊符号
对于Markdown中的语法符号，前面加反斜线 <mark>`\`</mark> 即可显示

## 数学公式
- 在两个<mark>`$`</mark>之间可直接在句子里显示公式
- 在两个<mark>`$$`</mark>之间可换行居中显示公式

## 制作待办事项
- 未完成：<mark>`- [ ] `</mark>
- 已完成：<mark>`- [x] `</mark>
e.g.
代码
```
- [ ] 某某某
- [x] 某某某某
```
$\quad\space\space$效果
- [ ] 某某某
- [x] 某某某某
