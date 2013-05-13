---
published: false
layout: post
title: "Aquamacs速查手册"
description: ""
category: 振导社会
tags: [emacs, 简介, 工具, 速查, 程序, lisp]
---
{% include JB/setup %}

[Aquamacs](http://Aquamacs.org)是[GNU Emacs](http://www.gnu.org/software/emacs/)在OSX系统的一个发行版，除了支持Emacs标准快捷键外，也支持与OSX系统兼容的快捷键。因此，本文的大多数内容对Emacs也实用。

>While the useful Emacs key bindings are supported and Emacs packages usually just work, Aquamacs is adapted to be comparable to younger text editors (than Emacs). It also comes with a range of packages that give you greater comfort in editing certain types of files.

##基础知识

###键盘对应关系

Emacs | Mac | PC
----|----|----
META | esc/option | Alt
CTRL | command | Ctrl

###获取帮助的快捷键

快捷键 | 功能
---- | ----  
`C-h t` | emacs tutorial 
`C-h i` | emacs info 
`C-h r` | emacs manual  
`C-h f <函数名>` | 函数的帮助信息  
`C-h v <变量名>` | 变量的帮助信息  
`C-h k <快捷键>` | 快捷键的帮助信息

Emacs最常用功能可通过简明的[GNU Emacs Reference Card](http://www.damtp.cam.ac.uk/user/eglen/ess11/resources/emacs-refcard.pdf)学习。

###info的快捷键

功能 | 快捷键 | 快捷键 | 功能
----: | ----: | :---- | :----
上一节点 | `p` | `n` | 下一节点   
上一菜单 | `[` | `]` | 下一菜单（菜单前标有*）
交叉引用跳转 | `f` | `l` | 交叉引用返回
向上翻页 | `DELETE` | `SPACE` | 向下翻页    
info tutorial| `h` | `C-l` | 当前节点循环翻页   
 上级菜单| `u` | `b` | 回到节首   
跳到指定节点 | `g` | `m` | 菜单   
回到info总目录  | `d` | `t` | 回到顶层菜单   

info兼容Emacs快捷键。

<!--
###Emacs戏法

 快捷键 | 功能
---- | ----
`C-u <数字> 快捷键`| 为快捷键传送参数
`M-<数字> 快捷键`| 执行快捷键`<数字>`次

如何输入`<数字>`个数字？

拼写检查：   
ispell、flyspell-mode

Tabs and Windows：
一个tab对应于一个buffer，OSX切换tab的快捷键是command+数字
Aquamacs配置文件的位置

可用C-h v load-path查看  
-->

###OSX系统和Emacs术语对应关系 

OSX Term | Emacs Term     
:--------|----------  
Window    | Frame      
Tab/pane  | Window    
Document  | Buffer    
Cursor    | Point    
Mouse pointer     | Pointer  
Keyboard shortcut | Key (binding)  

###Aquamacs相关文件 
插件目录： 
{% highlight bash %}
~/Library/Application Support/Aquamacs Emacs/myPlugin
{% endhighlight %}

配置文件目录：
{% highlight bash %}
/Library/Application Support/Aquamacs Emacs/  
/Library/Preferences/Aquamacs Emacs/  
~/Library/Preferences/Aquamacs Emacs/  
{% endhighlight %}

用户自定义配置文件：
{% highlight bash %}
~/.emacs
~/Library/Preferences/Aquamacs Emacs/Preferences.el
{% endhighlight %}

当然，用户可以在配置文件中指定目录和相关文件位置。

##Emacs Lisp基本语法（LISP = LISt Processing）

elisp是定义emacs配置文件的lisp方言。在lisp中，一切皆以括号`( )`的形式存在。

###数据类型

elisp 里的对象都是有类型的，而且每一个对象知道自己是什类型。内建的emacs数据类型称为primitive types，包括：整数、浮点数、cons、符号(symbol）、字符串、向量（vector）、散列表（hash-table）、subr（内建函数（比如cons、if、and之类）、byte-code function，和其它特殊类型，例如缓冲区（buffer）。

- 数字
	- 分为整数和浮点数两种类型
	- 运算符：`+``-``*``/``%``abs``mod`
	- 比较操作符：`<``>``>=``<=``=``/=`
	- 舍入函数：`truncate``floor``ceiling``round`
	- 其它常用函数：`sin``cos``sqrt``exp``log``random`
	
- 字符与字符串
	- 字符的读入语法是`?<字符>`
	- 常用的函数：`string``substring``string-to-number``up case``string-match`

- cons cell和list（列表）
	- cons cell由car和cdr两部分组成，car指向当前节点，cdr指向下一节点或`nil`，cons cell通过`.`将两部分分开（dotted pair）。
	- list按链表的形式存贮，每个节点是一个cons cell，`'(a b)`可由`'(a . (b . nil))`构建。
	- 列表前的单引号`'`告诉lisp不要对列表进行处理，如列表前没有单引号，列表的第一个元素是函数，`'`与`quote`函数等价，即`'(a b)`与`(quote (a b))`等价。
	- 生成一个cons cell用`cons`函数，它也是为list增加元素的方法。
	
- 序列和数组
	- 序列是列表和数组的统称，数组包括：vector（向量）、string（字符串）、char-table和bool-vector。
	- 数组的每个元素都对应一个下标，第一个元素下标为0；数组自求值；数组的元素都可用`aref`来访问，用`aset`来设置。
	- 向量是通用的数组，元素是任意对象；字符串是特殊的数组，元素只能是字符。
	- 创建向量可以用`vector`函数：`(vector 'foo 23 [bar baz] "rats")`。
	
- 符号
	- 符号是有名字的对象，相当于C语言的指针，符号名可含有任何字符。
	- 通过符号可以得到与符号相关的信息，比如值、函数、属性列表等。
	- 符号名唯一，在elisp中符号与名字通过obarray表关联。
	- `intern`函数查找或加入一个名字到obarray，返回对应符号，默认是全局obarray，也可指定obarray，`intern-soft`只是查找符号。当名字不在obarray时，`intern-soft`返回`nil`，而`intern`会加入符号到obarray中。
	- `unintern`函数去除在obarray中的符号，成功则返回`t`，未找到则返回`nil`。
	- lisp每读入一个符号都会`intern`到obarray中，在符号前加入`#:`可避免加入。
	- 符号有4个组成部分：
		- 名字，用`symbol-name`访问；
		- 值，用`set`设置，用`symbol-value`访问；
		- 函数，用`fset`设置，用`symbol-function`访问；
		- 属性列表，用`get`访问，用`put`修改，用`symbol-plist`得到所有属性列表。
	- `setq`是一个使用`set`的宏，`(setq sym val)`代表`(set (quote sym) val)`，q表示quoted，`setq`只能设置obarray中的变量。
	- `boundp`测试符号值是否设置过，如果是则返回`t`，否则返回`nil`；测试函数用`fboundp`。

###求值规则

一个要求值的lisp对象称为表达式（form），分为三种：符号、列表和其它类型。表达式的求值规则如下：

- 自求值表达式。包括：数字、字符串、向量，以及`t`和`nil`。
- 符号。符号的求值结果就是它的值，如果它没有值，抛出void-variable的错误。
- 列表表达式。根据第一个元素可分为：
	- 函数调用：第一个元素若为符号，解释器查找该符号的函数值，若其值仍为符号，继续查找，直到某个符号的函数值是一个lisp函数（lambda函数）、byte-code函数、原子函数（primitive function）、宏、特殊表达式或autoload对象，这一过程称为symbol function indirection。
	- 宏调用：第一个元素是宏对象，列表里的其它元素不会立即求值，先对宏扩展。
	- 特殊表达式：第一个元素是特殊表达式时，参数可能不会全求值，每个特殊表达式都有其对应的求值规则，如`cond`。
	
###变量

变量按作用域分为：全局变量、let绑定的局部变量和buffer-local变量。

全局变量无需声明，可以直接采用如下的方式赋值：

{% highlight cl %}
(setq foo "I'm foo")
{% endhighlight %}

另一个声明变量的方式是：

{% highlight cl %}
(defvar variable-name value
  "document string")
{% endhighlight %}

如果在声明之前，变量已经赋值，声明不会改变变量的赋值。

局部变量用`let`和`let*`进行绑定。let的绑定形式是：

{% highlight cl %}
(let (bindings)
  body)
{% endhighlight %}

例如：
{% highlight cl %}
(defun circle-area (radix)
  (let ((pi 3.1415926) erea)
    (setq area (* pi radix radix))
    (message "半径为%.2f的圆，面积是%.2f。" radix area)))
(circle-area 3)
{% endhighlight %}

`let*`与`let`唯一不同的是，在声明中就能使用前面声明的变量。使用方式如下：

{% highlight cl %}
(defun circle-area (radix)
  (let* ((pi 3.1415926)
	 (area (* pi radix radix)))
    (message "半径为%.2f的圆，面积是%.2f。" radix area)))
(circle-area 3)
{% endhighlight %}

局部变量的绑定不能超过一定的层数。

emacs有如此丰富的模式，各缓冲区之间相互不冲突，很大程度上归功于一个特殊的局部变量buffer-local。声明buffer-local变量的方法：

- `make-variable-buffer-local` 在所有的缓冲区都产生一个buffer-local变量。
- `make-local-variable` 所在的缓冲区产生一个局部变量。

在指定缓冲区执行用`with-current-buffer`，调用形式如下：

{% highlight cl %}
(with-current-buffer buffer
  body)
{% endhighlight %}

其中buffer既可是缓冲区对象也可是缓冲区名字，`get-buffer`可以用缓冲区名字得到缓冲区名字。

当一个符号作为全局变量有值，虽然用`make-local-variable`声明为buffer-local变量，但这个变量的值还是全局变量的值，这时全局变量的值也称为省却值，可用`defaule-value`访问。如果一个变量为buffer-local，这个缓冲区使用`sets`只能改变当前缓冲区的值，`setq-default`可以修改符号作为全局变量的值。测试变量是否为buffer-local可以用`local-variable-p`。在当前缓冲区获取其它缓冲区buffer-local变量用`buffer-local-value`：

{% highlight cl %}
(with-current-buffer "*Messages*"
  (buffer-local-value 'foo (get-buffer "*scratch*")))
{% endhighlight %}

变量命名的习惯：

- -hook：特定情况下调用的函数，如关闭缓冲区、进入某个模式；
- -function：值为一个函数；
- -functions：值为一个函数列表；
- -flag：值为`nil`或non-nil；
- -predicate：值为一个判断的函数，返回`nil`或non-nil；
- -program或-command：一个程序或shell命令；
- -form：一个表达式；
- -forms：一个表达式列表；
- -map：一个按键映射（keymap）。


###顺序执行 `progn`

按顺序执行`progn`的每个参数，并返回最后一个执行的值。调用形式如下：
{% highlight cl %}
(progn A B C ...)
{% endhighlight %}

###条件判断

`if`条件的定义形式：
{% highlight cl %}
(if condition
    then
  else)
{% endhighlight %}

`cond`分支跳转的定义形式：

{% highlight cl %}
(cond (case1 do-when-case1)
      (case1 do-when-case1)
      ...
      (caset do-when-caset))
{% endhighlight %}

除此之外，还有`when`和`unless`用于条件判断。

###逻辑运算符 `and` `or` `not`


###循环

`while`循环的结构如下：

{% highlight cl %}
(while condition
  body)
{% endhighlight %}

###函数

函数对象包括：

- 函数：这里特指用lisp写的函数
- 原子函数（primitive）：用C写的函数
- lambda表达式
- 特殊表达式
- 宏
- 命令：能用`command-execute`调用，函数也是命令

函数定义的形式：

{% highlight cl %}
(defun function (arguments-list)
  "document string"
  body)
{% endhighlight %}

每个函数都有一个返回值，这个返回值一般是函数定义里最后一个表达式的值。函数使用范例：

{% highlight cl %}
(defun 高斯 (玄)
  "计算从 1 到 '玄' 之间所有整数之和，包括这两个数。"
  (let ((总和 1)
	(保存玄 玄))
    (while (> 玄 1)
      (setq 总和 (+ 总和 玄)
	    玄 (- 玄 1)))
    (message "从 1 加到 %d 值为： %d" 保存玄 总和)))
(高斯 100)
{% endhighlight %}

函数参数列表的形式：

{% highlight cl %}
(REQUIRED-VARS...
 [&optional OPTIONAL-VARS...]
 [&rest REST-VAR])
{% endhighlight %}

把必须提供的参数写在前面，可选参数写在后面，最后用一个符号表示剩余的所有参数。当提供了剩余参数时，所有参数以列表形式存放。

通常的函数调用通过`eval`进行，有时需要在运行时才决定使用什么样的函数，这就需要`funcall`和`apply`，这两个函数都是把其余的参数作为函数的参数进行调用。`funcall`是直接把参数传递给函数，而`apply`的最后一个参数是一个列表，传入的参数把列表平铺后再传给函数。以下是二者的区别：

{% highlight cl %}
(funcall 'list 'x '(y) '(z))         ; => (x (y) (z))
(apply   'list 'x '(y) '(z))         ; => (x (y) z)
{% endhighlight %}

`lambda`表达式相当于匿名函数，定义方式是：

{% highlight cl %}
(lambda (arguments-list)
  "document string"
  body)
{% endhighlight %}

调用方式如下：

{% highlight cl %}
(funcall (lambda (name)
	   (message "Hello, %s!" name)) "Emacser")
{% endhighlight %}

也可以先把`lambda`表达式赋值给变量，然后再用`funcall`调用：

{% highlight cl %}
(setq foo (lambda (name)
	    (message "Hello, %s!" name)))
(funcall foo "Emacser")
{% endhighlight %}

递归函数的层数通过`max-specpdl-size`定义。

emacs运行时处于一个命令循环之中，不断从用户得到按键，然后调用命令来执行。lisp
编写的命令都含有一个`interactive`表达式指明了命令的参数。`interactive`参数的字符串的第一个代码字符代表了参数的类型，其后的字符串用于提示。如果命令有多个参数，可在提示字符串后用换行符分开，如下例：

{% highlight cl %}
(defun hello-world (name time)
  (interactive "sWhat's you name? \nnWhat's the time?")
  (message "Good %s, %s"
	   (cond ((< time 13) "morning")
		 ((< time 19) "afternoon")
		 (t "evening"))
	   name))
{% endhighlight %}

`interactive`用代码字符指定用户输入的方式可以用函数取代，比如`s`可用`read-string`代替如下：

{% highlight cl %}
(read-string "What's your name?" usr-full-name)
{% endhighlight %}


##Aquamacs常用配置

Aquamacs已经完成了很多Emacs的基本配置，配置中需要的相关elisp函数，[可查索引](http://www.gnu.org/software/emacs/manual/html_node/elisp/Index.html)。[ahei][]和[alexott][]的配置是不错的参考，[emacswiki][]和[emacser][]可以找到很多相关资料。

[el-get][]是一个与debian的`apt-get`类似的emacs配置包管理器。

[ahei]:http://code.google.com/p/dea/
[alexott]:https://github.com/alexott/emacs-configs
[emacser]:http://emacser.com/
[emacswiki]:http://www.emacswiki.org/   
[el-get]:https://github.com/dimitri/el-get


<!--
{% highlight cl %}
(set-default-font "Menlo Plus")
(set-fontset-font
 (frame-parameter nil 'font)
 'han
 (font-spec :family "Hiragino Sans GB"))
{% endhighlight %}

{% highlight cl %}
(set-fontset-font NAME TARGET FONT-SPEC &optional FRAME ADD)
{% endhighlight %}


- `NAME`
- `TARGET`
- `FONT-SPEC`可以通过函数`font-spec`设置，参数和`set-face-attribute`的一致
- `FRME ADD`

查看当前用的字体可以用`describe-font`
-->


###基本配置

加载配置文件的路径：

{% highlight cl %}
(add-to-list 'load-path "~/.emacs.d/site-lisp")
{% endhighlight %}

绑定快捷键到函数/命令：

{% highlight cl %}
(global-set-key (kbd "C-c c") 'comment-region-or-line) 
{% endhighlight %}


###代码补齐

代码补齐就是根据当前的输入字符，补全或提示即将要输入的字符。补齐的原理就是根据当前已输入的字符，在特定的字典里匹配出即将要输入的字符，因此，生成用于补齐的字典是关键。这个字典来源有两方面：一是，用户提前提前定义，[auto-complete][]的用户定义字典在dict目录，[yasnippet][]的代码片断预先定已在snippets目录，[emacs-template][]也有用于存放用户定义模板的templates目录；二是，程序自动分析生成，程序自动分析C/C++代码生成字典的有[cedet]和[gccsense][]，[rope][]可分析python代码生成补齐字典，还有一些自动生成补齐字典的方案与程序语言无关，通过当前缓冲区、文件等信息，自动生成补齐字典，比如[auto-complete][]和[company][]。

[auto-complete][]和[company][]主要功能是作为一个补齐前端，用户可以根据需要自定义用于补齐的后端驱动引擎，比如把[yasnippet][]（[解决company与yasnippet整合冲突的方案](http://www.emacswiki.org/cgi-bin/emacs-en/CompanyMode#toc8)）、[ropemacs][]和[cedet][]整合到它们的补齐方案中。[cedet][]（Collection of Emacs Development Environment Tools）结合[ecb][]（Emacs Code Browser）就是是一个功能完善的IDE方案，[cedet][]既有不错的前端，后端有有强大的C/C++代码解析引擎，[ecb][]方便阅读代码。

[yasnippet]:https://github.com/capitaomorte/yasnippet   
[emacs-template]:http://emacs-template.sourceforge.net   
[gccsense]:http://cx4a.org/software/gccsense/  
[ecb]:http://ecb.sourceforge.net/  

<!--
my-cedet-complete和company可以共存   
all-auto-complete-settings和company不可共存   
all-auto-complete-settings和my-cedet-complete不可共存   

Symbol's function definition is void: company-pysmell  
-->

[auto-complete]:http://cx4a.org/software/auto-complete/     
[company]: http://nschum.de/src/emacs/company-mode/   
[cedet]:http://cedet.sourceforge.net/  

###python的补齐方案
python的补齐方案基于[rope][]，python补齐首先需要安装[rope][]、[ropemode][]和[pymacs][]（安装时需要手动将Pymacs.py拷贝到python的site-packages目录）。在此基础上，可以安装[pysmell][]、[ropemacs][]或[pycomplete][]三者之一实现补齐。[company][]和[auto-complete][]这些补齐前端也可用[pysmell][]、[ropemacs][]作为其补齐的引擎。


[rope]:http://rope.sourceforge.net/   
[pycomplete]:http://www.rwdev.eu/articles/emacspyeng   
[pymacs]:https://github.com/pinard/Pymacs/   
[ropemode]:http://pypi.python.org/pypi/ropemode   
[pysmell]:http://code.google.com/p/pysmell/   
[ropemacs]:http://rope.sourceforge.net/ropemacs.html


##参考资源

[Aquamacs Emacs Manual](http://aquamacs.org/features.shtml)     
[emacswiki](http://www.emacswiki.org/)       
[GNU Emacs manual](http://www.gnu.org/software/emacs/manual/emacs.html)   
[叶文彬：Elisp入门](http://www.newsmth.net/bbsanc.php?path=%2Fgroups%2Fcomp.faq%2FEmacs%2Felisp%2Fhappierbee%2FM.1184679743.j0&ap=64311)  
[Elisp 编程](http://jianlee.ylinux.org/Computer/Emacs/elisp.html)     
[About Cons Cells](http://cs.gmu.edu/~sean/lisp/cons/)    
[wikipedia: S-expression](http://en.wikipedia.org/wiki/S-expression)      
[wikipedia: LISP](http://zh.wikipedia.org/wiki/LISP)  
[wikipedia: Lisp (programming language)](http://en.wikipedia.org/wiki/Lisp_(programming_language))   
[An Introduction to Emacs Lisp Programming](http://www.gnu.org/software/emacs/emacs-lisp-intro/)   
[GNU Emacs Lisp Reference Manual](http://www.gnu.org/software/emacs/manual/elisp.html)                
[Emacs补全利器：auto-complete+gccsense](http://emacser.com/emacs-gccsense.htm)   
[用CEDET浏览和编辑C++代码](http://emacser.com/cedet.htm)    
[手把手教你emacs cedet C/C++自动补全](http://www.cnblogs.com/logicbaby/archive/2011/10/19/2217253.html)   
[A Gentle introduction to CEDET](http://alexott.net/en/writings/emacs-devenv/EmacsCedet.html)   
[Emacs补全利器：auto-complete+gccsense](http://emacser.com/emacs-gccsense.htm)  

 
