---
published: false
layout: post
title: "python速查手册"
description: ""
category: 振导社会
tags: [python, 程序, 速查, 工具, 简介]
---
{% include JB/setup %}

python，一切皆对象！

python主要特色：解释型、面向对象、可扩展、可嵌入、小内核、动态类型、强类型。

##数据类型

- 动态类型：不需要事先声明变量类型；
- 强类型：一旦变量有了值，这个值就有一个类型，不同类型变量间不能直接赋值。

###整型（integer）、浮点型（float）

支持以下运算：

操作符|意义|例子|结果
:----:|:----:|:----:|:----:
+|加法|10 + 3|13
-|减法|10 - 3|7
*|乘法|10 * 3|30
/|除法|10 / 3|3
%|模|10 % 3|1
**|幂| 10 ** 3|1000
divmod|除法|divmod(10, 3)|(3, 1)


###None类型

###列表（list）

顺序存储结构，元素可为任意类型。形如：

{% highlight python %}
[ 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31 ]
[ 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday' ]
{% endhighlight %}

list的下标取值范围是`-len`到`len-1`，其中len为列表长度。列表lst的第一个元素是`lst[0]`或`lst[-len]`。提取含lst的2、3、4元素的字列表的方法是`lst[2 : 5]`，`lst[ : 5]`和`lst[2 : ]`的起始位和结束位分别为0、-1。

###元组（tuple）

元组是常量list，不能用pop，remove，insert等方法。

构造元组：
{% highlight python %}
a = (0, 1, 2, 3, 4)
a = 0, 1, 2, 3, 4
{% endhighlight %}

元组可同时对多个变量赋值：
{% highlight python %}
x, y = (4, 44)
x, y = 444, 4444
{% endhighlight %}

###字符串（string）

用单引号`'`或双引号`"`构成字符串。字符串的连接用`+`，字符串可视为字符构成的list。

格式化字符串的基本方法是`s % <tuple>`，格式由s确定，tuple为参数列表。基本格式化方法如下：

{% highlight python %}
print "%s’s height is %dcm" % ("Charles", 170)
# 返回：Charles’s height is 180cm
{% endhighlight %}

另一种格式化方法：
{% highlight python %}
print "%(name)s’s height is %(height)dcm" \
    ", %(name)s’s weight is %(weight)dkg." % \
    {"name":"Charles", "weight":70, "height":170}
# 返回：Charles’s height is 170cm, Charles’s weight is 70kg.
{% endhighlight %}

###序列（sequence）

sequence包括list、tuple和string。

- `+`表示连接两个序列；
- `*`表示重复一个序列；
- 序列支持list comprehension。

###字典（dictionary）

字典是无序存储结构，每个元素是一个pair。

- 包括key和value两部分，key的类型是integer或string，value的类型任意；
- 没有重复的key；
- 用D[key]的形式得到字典key对应的value。

构造dictionary的方法：
{% highlight python %}
pricelist = {"clock":12, "table":100, "xiao":100}
pricelist = dict([("clock",12), ("table",100), ("xiao",100)])
{% endhighlight %}

若对不存在的key赋值，则增加一个元素：
{% highlight python %}
pricelist["apple"] = 12
# price list = {’table’: 100, ’apple’: 12, ’xiao’: 100, ’clock’: 12}
{% endhighlight %}

读取不存在的key会抛出异常。若用`D.get(key)`，当D中有key则返回对应的value，否则返回None。`D.get(key)`相当于`D.get(key, None)`。

`D.items()`得到一个list，如下：
{% highlight python %}
a = {"four": 4, "three": 3, "two": 2, "one": 1}
a.items()
# 返回：[(’four’, 4), (’three’, 3), (’two’, 2), (’one’, 1)]
{% endhighlight %}

## 控制流程

###分支if

{% highlight text %}
if <expr1>:
    <statement-block>elif <expr2>:    <statement-block>elif <expr3>:    <statement-block>...else:    <statement-block>
{% endhighlight %}

- if后可跟任何表达式，除None、0、\"\"（空字符串）、[]（空list)、()（空tuple）和{}（空dictionary）外，其他都为真；
- `switch`结构通过`elif`实现。

###循环for、while、break、continue
for语法：
{% highlight text %}
for x in <sequence>:
    <statement-block>
else:
    <else-block>
{% endhighlight %}
`else`部分可有可无。若有，则表示每个元素都循环到了（无`break`），那么就执行\<else-block\>。

while语法：
{% highlight text %}
while <expr1>:
    <block>
else:
    <else-block>
{% endhighlight %}
在循环结束时若expr1为假，则执行\<else-block\>。

##函数

语法：
{% highlight text %}
def <function_name> ( <parameters_list> ):
    <code block>
{% endhighlight %}

- 函数没有返回值类型，`return`可以返回任何类型；
- 函数名称只是一个变量，可把一个函数赋值给另一个变量。

###个数可变参数

例如：
{% highlight python %}
def printf(format, *arg):
    print format%arg
{% endhighlight %}
`*arg`必须为最后一个参数，`*`表示接受任意多个参数，多余的参数以tuple形式传递。

另一种用dictionary实现可变个数参数的方法：
{% highlight python %}
def printf(format, **keyword):
    for k in keyword.keys():
        print "keyword[%s] is %s"%(k,keyword[k])
{% endhighlight %}
`**keyword`表示接受任意个数有名字的参数。

`*arg`和`**keyword`同时存在时，`*arg`要在`**keyword`之前。

###Doc String函数描述

每个函数对象都有一个`__doc__`属性，如第一表达式是string，函数的`__doc__`就是这个string，否则`__doc__`为None。
{% highlight python %}
def testfun():
    """
    this function do nothing, just demonstrate the use of the doc string.
    """
    passprint testfun.__doc__
#返回：this function do nothing, just demonstrate the use of the doc string.
{% endhighlight %}
用这种方法写帮助文档，有利于程序和文档的一致性。

###lambda函数

lambda函数是一种匿名函数。
{% highlight python %}
f = lambda a, b: a + b
f(1, 2)
#返回：3
{% endhighlight %}

###嵌套函数

{% highlight python %}
def outfun(a, b):
    def innerfun(x, y):
 	    return x + y
    return innerfun(a, b)
{% endhighlight %}

##python特色

###list comprehension

语法：
{% highlight text %}
[ <expr1> for k in L if <expr2> ]
{% endhighlight %}

语义：
{% highlight python %}
returnList=[]
for k in L:
    if <expr2>: returnList.append(<expr1>)
return returnList;
{% endhighlight %}

例如：
{% highlight python %}
a = ["123", "456.7", "abc", "Abc", "AAA"]

[ k.upper() for k in a if k.isalpha() ]
# 返回：['ABC', 'ABC', 'AAA']

[ int(k) for k in a if k.isdigit() ]
# 返回：[123]
{% endhighlight %}

###模块（module）和包（package）

一个module就是一些函数或类放在一个文件中。package是一组module在一个文件夹的集合，该文件夹须包含一个\_\_init\_\_.py文件。

##类

##异常处理

##文档与注释
