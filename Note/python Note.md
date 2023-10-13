# python Note
## 基础
- 输入字符串应在<mark>`' '`</mark>或<mark>`" "`</mark>环境内
- <mark>`print( ),input()`</mark>
- <mark>`f"{ }{ }"`</mark>连接字符串及其他内容
    - f为format缩写
    - 注:可用以换行切断长句子
- <mark>`<argument>.title()`
`<argument>.upper()`
`<argument>.lower()`</mark>
- <mark>`\t`</mark>纵向制表符;<mark>`\v`</mark>横向制表符;<mark>`\n`</mark>换行
- <mark>`<arguement>.rstrip()`</mark>去除末尾字符；
<mark>`<argument>.lstrip()`</mark>去除首部字符
<mark>`<arguement>.strip()`</mark>去除两头字符
- 常量建议用全大写表示
- 添加注释
    - `#`
    - `"""___"""`
    - `'''___'''`
- 长代码换行
	- 借助注释符`'''___'''`(`"""___"""`):(用于`print()`)代码换行,内容也换行,缩进计入空格
	- 借助转义符`\`:行末输入'`\`',然后另起一行
    	    - 若在`print()`内使用,内容不换行,缩进计入空格
	- 闭合操作符(即三种括号):只涉及单一语句时有效
- `=`等于,`!=` or `<>`不等于,`>`大于,`<`小于,`>=`大于等于,`<=`小于等于,`%`求模
- 可用<mark>`and`</mark>连接多个条件(同时满足才为`True`)
<mark>`or`</mark>也可连接多个条件(至少有一个满足即可)
- `int()`整点数,`float()`浮点数
- `while`,`True`,`False`,(`flag`)
- <mark>`break`</mark>：退出循环
<mark>`continue`</mark>：忽略余下代码,返回循环开头
<mark>`pass`</mark>：空操作语句,起占位作用,跳入下一条语句

## 列表
- 定义：<mark>`<list>=[<element_1>,<element_2>,...]`</mark>,提取列表元素应用`[num]`指名编号
    - <font color=red>ATTENTION</font>：编号从0开始！！！
- 列表最后一个元素可用编号<mark>`[-1]`</mark>，可以此倒退
- 确定列表长度:<mark>`len(<list>)`</mark>
- <mark>`in`,`not in`</mark>检查特定值是否在列表中
### 插入元素
- <mark>`<list>.append()`</mark>：用于列表末尾附加元素
- <mark>`<list>.insert(<num>,<element>)`</mark>：插入元素
### 删除元素
- `del` 语句(delete)：<mark>`del <list>[num]`</mark>  (之后不能继续使用该元素)
- `pop`弹出：<mark>`<list>.pop()`</mark>弹出最后一个元素即只显示最后一个元素(之后可以继续使用该元素)
- `remove`：<mark>`<list>.remove(<element>)`</mark>利用<mark>元素名称</mark>移除元素而无需知其编号(之后仍可继续利用它)
若要一次移除多个，可利用for循环语句
e.g.
```python
A=['a','b','c','d','e']
B=['a','b','c']
C=['a','b','c','d','e']
for x in A:
	if x in B:
	C.remove(x)
print(C)
```
### 列表排序
- 永久固定
	- <mark>`<list>.sort()`</mark>正序
	- <mark>`list>.sort(reverse=True)`</mark>逆序
- 临时排序:<mark>`sorted(<list>,参数)`</mark>
    - 供选参数
        - <mark>`reverse=True/False`</mark>,默认为`False`
        - <mark>`iterable=`</mark> ,可迭代的数据(容器类型数据,`range`数据序列,迭代器)
        - <mark>`key=<func>`</mark>,可选自定义函数
- 倒着打印列表:<mark>`<list>.reverse()`</mark>,永久性直接倒转列表,无规定顺序,<u>再次用`.reverse()`可恢复原序</u>
### *列表解析
将`for`循环和创建新元素的代码合并成一行，并自动附加新元素(此时`for`后无冒号)
e.g.
```python
squares=[value**2 for value in range(1,11)]
```
### 列表切片
- <mark>`<list>[m:n]`</mark>意为从编号为`m`的元素开始到编号为`n-1`的元素引用列表,`m`默认为`0`,`n`默认为`-1`
- 可用`for`循环语句遍历切片
- 复制列表切片
e.g.
    ```python
    A=['a','b','c','d','e']
    B=A[1:3]
    ```
	- 可用`<list>[:]`创建列表副本

## 元组
tuple：与列表类似,区别在于`tuple`用`()`且<mark>不可修改</mark>(但可重新给变量赋值),而`list`用`[]`且可修改
<font color=red>ATTENTION</font>:严格地说，元组是由逗号标识的，圆括号只是让元组看起来更整洁清晰，若要定义只包含一个元素的元组，必须在这个元素后面加上逗号
## 字典
dictionary:一系列键值对的集合
- 格式:<mark>`<dic>={<key_1>:<value_1>,<key_2>:<value_2>,...}`</mark>
- 访问字典:<mark>`<dic>[key]`</mark>
- 可将较大的字典、列表、元组在每个","后拆分为多行
- 使用<mark>`<dic>.get(<?key>,<value>)`</mark>访问`<dic>`中的值可避免`<?key>`不在`<dic>`中而引发`KeyError`,此时`<?key>:<value>`
- 嵌套使用字典与列表
### 键值对的添加与删除
- 添加:<mark>`<dic>[<new_key>]=<new_value>`</mark>
    - 也可用此法更改字典中原有的键值对
- 删除:<mark>`del <dic>[<key>]`</mark>
### 遍历字典
- 所有键值对:<mark>`for循环语句+方法<dic>.items()`</mark>,此时需要两个变量
- 所有键:
	- 原序:<mark>`for循环语句+方法<dic>.keys()`</mark>
	    - `for`循环`<dic>`默认为`keys`
	- 特定顺序:`sorted()`
- 所有值:<mark>`for循环语句+方法<dic>.values()`</mark>
	- 若要使输出值不重复,可用集合<mark>`set(<dic>.values())`</mark>
## 集合
set:<mark>`<set>={<element_1>,<element_2>,...}`</mark>
## 函数与模块
- 函数是实现一个代码效果的代码块
- 模块是包含变量、常量、函数、列表、字典、类等定义的`.py`文件
### 常用函数
- <mark>`range()`</mark>
    - 一个参数`n`:从`0`开始数`n`个数
    - 两个参数`(m,n)`:从`m`到`n-1`
    - 三个参数`(m,n,t)`:以`t`为步调从`m`到`n-1`
    - <mark>`list(range())`</mark>可用于创建数字列表
- <mark>`max()`,`min()`,`sum()`,`pow(<num>,<index>)`</mark>
- <mark>`random`</mark>随机数:(<mark>`import random`</mark>)
	- <mark>`random.random()`</mark>：生成一个`0`到`1`的随机浮点数
	- <mark>`random.uniform(m,n)`</mark>：生成一个在`m`与`n`间的随机浮点数
	    - `m`,`n`二者无需讲究大小顺序
	- <mark>`random.randint(m,n)`</mark>：生成一个在`m`与`n`间的随机整数,其中$m<n$
	- <mark>`random.choice(<seq>)`</mark>：从非空序列`<seq>`中随机获取一个元素,<mark>`<seq>=<list/tuple/str/set>`</mark>
    - <mark>`random.choices(<population>,weights=<relative_weight_list>,cum_weights=<cumulative_weight_list>,k=<num>)`</mark>
	    - `weights`与`cum_weights`可只存在一个,最好用`weights`
	    - 放回取`k`次
    - <mark>`random.sample(<seq>,<num>)`</mark>：从指定序列中随机获取指定长度,但是长度不能大于生成个数,且为不重复的随机数,不改原有序列
	    - 不放回取`<num>`次
    - <mark>`random.randrange(m,n,<step>)`</mark>：从`[m,n]`中以`<step>`为步调取随机数
    - <mark>`random.shuffle(<list>)`</mark>：将`<list>`重新打乱,不生成新列表
### 定义函数
```python
def <func>(<parameter>):
""""<instruction>"""
<define>
```
__注__:`<func>`后的括号必不可少,第二行注释可省,如需则必须在三个双引号内
### 形参和实参
#### 传递多个实参
- 位置实参：一 一对应,顺序不可调,$f(x,y,z,...)$
- 关键词实参：多个实参的位置可变,$f(x= ,y= ,z= ,...)$
- 给形参设定默认值：$f(x,y,z=C,...)$
	- 此时设定默认值的形参必须放在最后,其余形参可用上述两方法调用
	- 此时仍可在调用函数时更改默认值
#### 设置形参和实参
- 若不清楚`<func>`共需几个`<parameter>`,可设为<mark>`<func>(<*parameter>)`</mark>,此时所有`<parameter>`都将存在<mark>元组`<*parameter>`</mark>内
	- 可与位置实参混用
- 如需使用任意数量的关键词实参,可设为<mark>`<func>(<**parameter>)`</mark>，此时建立一个<mark>名为`parameter`的空字典</mark>
### 导入模块/函数
- <mark>`import <module>`</mark>:导入整个`<module>`,此后每次使用模块中的函数时结构都应为<mark>`<module>.<func>()`</mark>
- <mark>`from <module> import <func_1>,<func_2>,...`</mark>:导入`<module>`中的特定函数`<func_1>,<func_2>,...`,此后可直接使用该函数
- <mark>`from <module> import *	`</mark>:导入`<module>`中的所有函数`<func>`,此后可直接使用函数而无需句点表示法
	- <font color=red>ATTENTION</font>:若`<module>`非自定义,最好不要用此法,以免函数名或变量名等发生重复而导致出错！！！
#### 路径相关
- 同一文件夹:直接`import`
- `<module>`在同一大文件夹`<file>`的子文件夹`<sub_file>`内:<mark>`import <sub_file>.<module>`</mark>
- 任意路径:
```python
import sys  #导入系统模块
sys.path.append(r"<load>")	#将<module>导入sys中
import <module>
```
### 指定别名
<mark>`as`</mark>对`<module>/<func>`指定别名/简写缩写:
```python
import <module>/<func> as <NewName>
```
此后只需使用`<NewName>`即可有同等效力
### return
- 返回值,其后代码不翻译
- 记录值而不立即打印
- <mark>`return <argument_1>,<argument_2>,... `</mark>：返回一个元组
- <font color=red>ATTENTION</font>:`return`只能在`<func>`中使用！！！
## 类
class:将对象作为实例存储在类中,并能后续调用类的内容
(`<Calss>`中的`<func>`称为方法)
```python
class <Class>:	#约定类名每个首字母大写且不用"_"
	def __init__(self,<attribute_1>,<attribute_2>,...):
	    #方法__init__()与形参self必不可少且self在形参之首
		self.attribute_1=attribute_1
		self.attribute_2=attribute_2
		...		#将其他属性<attribute>导入self中
	def <func>(self,<parameter>):	
	#其他形参可以无,self必须有
	    ...
```
<font color=red>ATTENETION</font>:对于属性`<attribute>`,约定名称前有两个下横线(<mark>`__`</mark>)的为<mark>私有属性</mark>,后续不可直接修改,只能借助方法间接修改
### 根据类创建实例
<mark>`<example>=<Class>(<attribute_1>,<attribute_2>,...)`</mark>
- 访问属性:<mark>`<example>.<attribute>`</mark>
- 调用方法:<mark>`<example>.<func>(<parameter>)`</mark>
    - 只用填写处`self`外的其他实参
    - 从属性的角度看,类也可看作多个实例的字典所组成的集合
### 修改`<example>`的`<attribute>`
- 直接修改:<mark>`<example>.<attribute>=<NewAttributeValue>`</mark>
- 借助方法
### 类的继承
- 子类继承父类:
```python
class <Subclass>(<Superclass>):
	def __init__(self,<attribute>):
	    super().__init__(self,<attribute>)
	    #调用父类的属性和方法
		self.<sub_attribute>=<sub_attribute>
		#设置子类的特有属性
```
- 重写父类的方法:在子类中重新定义与之同名的方法
- 将实例用作属性:当类的属性过多时,可将部分提出出来作为独立的类,再将这个小类作为大类的属性
```python
class <Sub_class>:
	def __init__(self,<sub_attribute>):
		...
	...
class <Class>:
	def __init__(self,<attribute>):				    
	    self<A_attribute>=<Sub_class>(<sub_attribute>)
		...
	...
<example>=<Class>(...)
<example>.<Sub_calss>.<attribute/func>
```
### 导入类
- <mark>`from <module> import <Class_1>,<Class_2>,...	`</mark>:从一个`<module>`导入部分`<Class>`
- <mark>`import <module>`</mark>:导入整个`<module>`,此后引用类应用句点表示法
- <mark>`from <module> import *`</mark>:导入`<module>`中的所有`<Class>`
    - 不推荐,同导入函数/模块