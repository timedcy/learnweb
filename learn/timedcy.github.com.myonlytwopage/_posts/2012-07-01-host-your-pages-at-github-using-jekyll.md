---
published: false
layout: post
title: "基于jekyll的github建站指南"
description: ""
category: 振导社会
tags: [github, git, jekyll, 速查, 简介, 程序]
---
{% include JB/setup %}

简介：本文主要介绍利用jekyll在github上搭建站点的方法与原理。

<span id="jekyll-and-github"></span>

##jekyll与github的关系
github站点是位于username.github.com仓库中的静态页面。利用高效的工具生成高质量的静态页面是github建站的王道。[jekyll](http://jekyllrb.com/)就是用[ruby](http://www.ruby-lang.org/)[制造静态站点的的ruby解析引擎](http://jekyllbootstrap.com/lessons/jekyll-introduction.html)（构建网站时ruby不是必会的）。github除了能构建独立的站点外，还可以为每个项目建立站点。github提供了[自动](https://help.github.com/articles/creating-pages-with-the-automatic-generator)和[手动](https://help.github.com/articles/creating-project-pages-manually)建立项目站点的方法。

github标配有jekyll解析引擎，上传的站点可以代由github的jekyll解析为静态页面。如果[不需要github用jekyll处理上传的站点文件](https://help.github.com/articles/files-that-start-with-an-underscore-are-missing)，可以在根目录下添加一个.nojekyll文件。

[github使用jekyll的\_config.yml](https://help.github.com/articles/using-jekyll-with-pages)和用户本地jekyll的不同，它的功能有限，github运行jekyll带有`--safe`参数。如果用户本地的jekyll使用了自定义插件，github的解析会出错，启用了本地自定义插件的用户可以上传本地解析好的静态页面到github。

由于本地和gihub的[jekyll版本](https://help.github.com/articles/using-jekyll-with-pages)不同，即使本地正常解析的页面也可能会在github上解析出错。下面的表格在github上`Jekyll 0.11.0 with Liquid 2.2.2`会出错：

{% highlight bash %}
   功能  |   快捷键  |   快捷键  |  功能
-------: | -------: | :------- | :-------
上一节点  |   `p`    |    `n`   | 下一节点  
||    `m`   | 菜单 
{% endhighlight %} 
  
##jekyll驱动网站原理

[用jekyll构建静态网站](http://chen.yanping.me/cn/blog/2011/12/15/building-static-sites-with-jekyll/)很容易。模板、数据和美化是jekyll建站的三要素。jekyll自动将数据注入模板，通过美化，得到站点。这里可以看到[一堆用jekyll打造的站点](https://github.com/mojombo/jekyll/wiki/Sites)，大部分都可以`git clone`回来学习参考。  
<!--more-->

###jekyll的模板
jekyll模板就是带有变量的html格式文件。jekyll的模板用[Liquid](http://liquidmarkup.org/)语言描述，定义了页面框架。Liquid通过（Output）和标签（Tag）两种标记（markup），即可描述页面的结构，具体可参阅[Liquid for Designers](https://github.com/Shopify/liquid/wiki/Liquid-for-Designers)。jekyll对Liquid的标签进行了[扩展](https://github.com/mojombo/jekyll/wiki/Liquid-Extensions)。Liquid模板语言对模板数据描述（当然也需要html），生成了站点页面的骨架。Liquid所需要的jekyll的[模板数据][TD]在这儿可以找到。除此之外，[YAML Front Matter][YFM]定义的变量也可以作为模板数据。

[TD]: https://github.com/mojombo/jekyll/wiki/template-data   
[YFM]: https://github.com/mojombo/jekyll/wiki/YAML-Front-Matter

###jekyll的数据
jekyll的数据是用[markdown](http://daringfireball.net/projects/markdown/)、[textile](http://textile.sitemonks.com/)等标记语言写的文档。这些格式的文档解析为html格式后，注入jekyll模板的<code>&#123;&#123; content &#125;&#125;</code>，就有了网站的页面。jekyll支持的markdown解析器（将markdown转换为html）有：<span id="markdown"></span>[rdiscount](https://github.com/rtomayko/rdiscount/)、[kramdown](http://kramdown.rubyforge.org/)、[redcarpet](https://github.com/tanoku/redcarpet/)、[maruku][maruku]（jekyll默认）、[bluecloth](http://deveiate.org/projects/BlueCloth/)。为了方便处理$$\LaTeX$$公式，也有人hack了jekyll，[将pandoc作为markdown的解析器](http://yangzetian.github.com/2012/04/15/jekyll-pandoc/)。一个更方便的方法是通过[jekyll-pandoc-plugin](https://github.com/dsanson/jekyll-pandoc-plugin)插件，启用pandoc解析器。这些hack后的jekyll启用pandoc当然不会在github上生效，只能用于本机解析后，上传静态页面。

[maruku]: http://maruku.rubyforge.org/

###jekyll的美化
美化就是用CSS和javascript对html描述的页面进行渲染，美化网页。[twitter bootstrap](http://twitter.github.com/bootstrap/)是一套极易上手的页面美化工具。twitter bootstrap还提供了[960网格布局](http://960.gs/)，只要按照它约定的方式对页面结构定义、对html标签的`class`命名，网页的布局美化可谓快又好。


##在jekyll-bootstrap基础上搭建网站

jekyll是一套根据相应规程生成网站的工具。如果按照这套法则，从头开始仍然麻烦。[jekyll-bootstrap](http://jekyllbootstrap.com/)就是一个jekyll构建站点的demo，并提供了一些[额外的api增强jekyll的功能](http://jekyllbootstrap.com/api/bootstrap-api.html)，在此基础上构建站点相当快捷容易。除此之外，[octopress](http://octopress.org/)也是基于jekyll上的不错的工具套件。

###目录结构
jekyll-bootstrap的[目录结构](https://github.com/mojombo/jekyll/wiki/usage)中，只需要将markdown文档放到\_posts目录下，`jekyll --server`，即可在http://localhost:4000看到页面内容，见[快速入门示范](http://jekyllbootstrap.com/usage/jekyll-quick-start.html)。\_posts中的数据文档，通过注入\_layouts定义的模板（\_layouts的模板一般指向了\_includes/themes中的模板），渲染页面的CSS和JS文档在assets/themes中，一些解析markdown用到的ruby插件放在\_plugins目录。通过`jekyll --server`最终生成的静态页面在\_sites目录。在更新github上站点的时候，`git push`可以不必推送_sites目录，将页面的解析留给github，这样做的前提条件是本地未启用自定义插件，\_config.yml设置`safe = true`进行解析。如果本地启用了自定义插件，解析任务就不能交给github做了，上传本地解析好的静态页面到github即可。

jekyll-bootstrap的\_includes/JB中有一些常用的工具，用于列表显示、评论等，可参看\_includes/themes中主题的相关html文档学习。

\_includes/themes中的主题一般包含default.html、post.html和page.html三个文档。default.html定义了网站的最上层框架（模板），post.html和page.html是其子框架（模板）。[post和page有别](http://jekyllbootstrap.com/lessons/jekyll-introduction.html#posts_and_pages)；`rake page`命令生成的页面按page.html解析，markdown文件的[YAML Front Matter][YFM]中`layout: page`；`rake post`命令生成的页面按post.html解析，markdown文件的[YAML Front Matter][YFM]中`layout: post`。生成好的html子页面通过default.html的<code>&#123;&#123; content &#125;&#125;</code>变量调用，生成整个页面。

###自定义
站点生成需要用到\_config.yml[配置文件](https://github.com/mojombo/jekyll/wiki/configuration)，`markdown`变量定义了解析markdown用的[解析器](#markdown)。当然也可自定义变量，比如在`JB:`下定义一个二级变量`RSS_path: /atom.xml`（注意缩进）。在html页面中，就可以用<code>&#123;&#123; site.JB.RSS_path &#125;&#125;</code>得其值。[YAML Front Matter][YFM]中也可以自定义变量，其中的`title`变量可以用<code>&#123;&#123; page.title &#125;&#125;</code>访问到。也就是说，站点的全局变量在\_config.yml中定义，用`site.`访问，页面的变量在[YAML Front Matter][YFM]中定义，用`page.`访问，更多的模板变量可参考[模板数据][TD]。

\_includes/JB中的插件可以在\_includes/custom中重新自定义，方法可仿照\_includes/JB中的。如果要\_includes/custom中的插件生效，需要在\_config.yml中将相应的变量设为`custom`，设置线索可见\_includes/JB中的插件。

###小结
[jekyll生成静态页面的过程](http://jekyllbootstrap.com/lessons/jekyll-introduction.html#how_jekyll_generates_the_final_static_files)就是先搜集站点的原始数据，通过计算，生成用于页面显示的结构化数据（最终的html静态页面）。结构化数据来源有两方面：一方面是文件中的配置文件，如\_config.yml和markdown的[YAML Front Matter][YFM]；另一方面通过目录结构和jekyll定义的解析法则，产生数据。站点的全局数据用`site.`访问，页面数据用`page.`访问。有了这些结构化的数据后，jekyll再按照用户定义的模板，用相应的结构化数据替换模板中的变量。用户编写的插件是为了计算出满足用户特定需求的数据，但是这些自定义插件不能不github的jekyll解析支持。

##jekyll的装饰部件
这些装饰部件中，如果本地的jekyll使用了自定义的插件，github的jeklly解析器不会支持这些自定义插件。

###图片与文件
站点需要的图片直接存放在`assets/images`文件夹中，可以在\_config.yml中定义一个形如`img_url: http://jiyeqian.github.com/assets/images`的变量，然后在markdown文件需要用到图片的地方插入类似<code>![git代码库结构](&#123;&#123; site.img_url &#125;&#125;/2012-06-27-git-transport.png)</code>的代码即可显示图像。

如果图和文件较多，这里有一些[图片和文件托管的网络资源]({{ site.production_url }}/2011/09/web-cloud-life/)供参考。

###代码高亮
网页代码高亮的一般原理是先用JS或其它脚本语言对代码的进行解析，并提根据不同程的序语言提取关键字、变量、常量、注释，然后将代码层次结构用html标签描述，最后用CSS着色。

代码高亮的方案有很多，比如：[转帖gist代码的插件](https://gist.github.com/1027674)，[google-code-prettify](http://google-code-prettify.googlecode.com/svn/trunk/README.html)，[利用pygments高亮代码的插件](https://github.com/rsim/blog.rayapps.com/blob/master/_plugins/pygments_cache_patch.rb)，octopress的[backtick-codeblock](http://octopress.org/docs/plugins/backtick-codeblock/)，如果需要在线展示html、js、CSS及其效果，当然还是用[jsfiddle插件](http://octopress.org/docs/plugins/jsfiddle-tag/)。

pygments是github可支持的代码高亮方案。利用pygments，须先在\_config.yml中设置`pygments: true`，并嵌入生成的相应CSS到页面的`<head></head>`之中。   
{% highlight bash %}
$ pygmentize -S default -f html | sed 's/^/.highlight code /g' > default.css
{% endhighlight %}   

在pygments的CSS选择器前都加上`.highlight code`，防止pygments的CSS影响[mathjax](#mathjax)公式的CSS。[pygments也可能会和bootstrap.min.css冲突](http://www.stehem.net/2012/02/14/how-to-get-pygments-to-work-with-jekyll.html)，需要修改css。上面的`pygmentize`命令就是pygments代码高亮的效果。代码高亮的highlight标签使用方法[可以参考这里](https://github.com/mojombo/jekyll/wiki/Liquid-Extensions)。

另一个比较特殊的代码高亮插件是[include-code](http://octopress.org/docs/plugins/include-code/)，可以直接显示目录中的代码文件，在\_config.yml中设置好`code_dir`参数后，直接用<code>&#123;% include_code excerpt.rb %&#125;</code>，即可显示高亮代码（布局可自己定义，代码高亮的CSS可以用pygments的）。[pygments也支持类似的方法](https://gist.github.com/2890453#working-with-code-partials)，但效果不太好。

<span id="mathjax"></span>

###$$\LaTeX$$公式
github可支持的$$\LaTeX$$方案需要绕过自定义插件，利用类似[mathjax](http://www.mathjax.org/)的javascript方案或采用支持$$\LaTeX$$的markdown解析器（比如[maruku][maruku]）。

[mathjax](http://www.mathjax.org/)通过javascript的方式生成$$\LaTeX$$公式可以参考[在Octopress中使用$$\LaTeX$$](http://chen.yanping.me/cn/blog/2012/03/10/octopress-with-latex/)，不过，不必把markdown解析引擎设置为kramdown。当markdown解析引擎为kramdown时，只需要在页面中插入下面这段代码：
{% highlight html %}
<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
{% endhighlight %}
下面这段代码：
{% highlight html %}
$$ 
e^x = \sum\_{n=0}^\infty \frac{x^n}{n!} = \lim\_{n\rightarrow\infty} (1+x/n)^n 
$$
{% endhighlight %}
显示的效果：
$$ 
e^x = \sum\_{n=0}^\infty \frac{x^n}{n!} = \lim\_{n\rightarrow\infty} (1+x/n)^n 
$$
显示行内公式$$\alpha + \beta$$的代码如下：
{% highlight html %}
$$\alpha + \beta$$
{% endhighlight %}
但是kramdown的解析引擎和转帖gist代码的插件、直接高亮文件中代码的插件include-code有冲突。

利用[mathjax插件](https://gist.github.com/834610)定义的新标签，也可插入$$\LaTeX$$公式。这段代码<code>&#123;% math %&#125; e&#94;x = \sum\_{n=0}&#94;\infty \frac{x&#94;n}{n!} = \lim\_{n\rightarrow\infty} (1+x/n)&#94;n &#123;% endmath %&#125;</code>也可显示和上面一样的公式，利用标签<code>&#123;% m %&#125; \alpha + \beta &#123;% em %&#125;</code>可显示行内公式$$\alpha + \beta$$。

另外一些支持$$\LaTeX$$公式的方案可参考：Kramdown的[Math Blocks](http://kramdown.rubyforge.org/syntax.html#math-blocks)、[Maruku的公式支持](http://maruku.rubyforge.org/math.xhtml)。 其他和学术写作相关的插件有：[jekyll-scholar](https://github.com/inukshuk/jekyll-scholar)、[jekyll-citation](https://github.com/archome/jekyll-citation)、[bibjekyll](https://github.com/pablooliveira/bibjekyll)。

###其它
显示相关文件的插件用[related_posts-jekyll_plugin](https://github.com/LawrenceWoodman/related_posts-jekyll_plugin)、[只显示第一段](https://github.com/sebcioz/jekyll-only_first_p)的插件，可解析类似wordpress的`<!--more-->`标记的[excerpt插件](https://gist.github.com/986665)，美化引用格式的[blockquote插件](http://octopress.org/docs/plugins/blockquote/)。[octopress的插件](http://octopress.org/docs/plugins/)可以直接用于jekyll中。

###小结
要将jekyll的页面托管在github时，推荐尽量不用github之外的插件。代码高亮用pygments，markdown的解析器用kramdown更利于对公式的解析，如果还要其它一些美化，就直接插html代码吧！

##一些技巧

###直接插入gist代码
因为markdown的语法可以兼容html，在情非得已的情况下，直接查html代码。如果转帖gist的代码，直接在markdown文件中插入：
{% highlight html %}
<script src="https://gist.github.com/834610.js?file=Jekyll nd Octopress Liquid tag for MathJax.rb"></script>
{% endhighlight %}
即可得：
<script src="https://gist.github.com/834610.js?file=Jekyll nd Octopress Liquid tag for MathJax.rb"></script>

###页内跳转
为了方便页内快速跳转，可建立空内容的锚点。比如
{% highlight html %}
<span id="jekyll-and-github"></span>

##jekyll与github的关系
{% endhighlight %}
既可用
{% highlight html %}
[回去看看](#jekyll-and-github)
{% endhighlight %}
实现页内跳转，试试，[回去看看](#jekyll-and-github)。如果用的是kramdown解释器，上例中空锚点行下须有一空行。

###表格不断行
表格某一栏不断行的处理：
{% highlight html %}
<span style="white-space:nowrap">git clone username@host:/path/to/repository</span>
{% endhighlight %}
点击[到这里看效果]({{ site.production_url }}/2011/09/introduction-to-git/)。


