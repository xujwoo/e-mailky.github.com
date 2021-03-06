---
layout: post
title:  x86实模式到保护模式及Linux启动协议的演变
categories: [Linux]
tags: [Linux, C, Kernel]
description: ""
---

## Markdown是什么
&emsp;&emsp;&emsp;&emsp;Markdown是一种轻量级标记语言，它允许人们“使用易读易写的纯文本格式编写文档，
然后转换成有效的XHTML(或者HTML)文档”。

[作者：约翰.格鲁伯 (John Gruber)，这里是他博客上](http://daringfireball.net/projects/markdown/syntax)
![约翰.格鲁伯](/images/kernel/figure1.gif)
![作者：亚伦.斯沃兹 (Aaron Swartz) (已故)](http://e-mailky.github.io/images/2015/20140325110340703.jpg)

## 为什么使用Markdown
* 很多人在用，各大主要实现的开源工程非常活跃。
* 简洁，提高文档撰写效率。
* 只与文档内容有关，与文档样式无关，便于调整样式。
* 基于纯文本格式，便于搜索。
* 有丰富的解释器或工具支持。

## 标准语法
# 强调
_斜体_  
{% highlight ruby %}
*斜体*
_斜体_
{% endhighlight %}
**粗体**
{% highlight ruby %}
**粗体**
__粗体__
{% endhighlight %}

# 换行
在行末输入两个空格然后回车即可实现文本的强制换行。  
# 标题
主标题
-
# H1标题
## H2标题
### H3标题
#### H4标题
##### H5标题
###### H6标题
{% highlight ruby %}
主标题
-
主标题
----------
主标题
=
主标题
==========
# H1标题
## H2标题
### H3标题
#### H4标题
##### H5标题
###### H6标题
{% endhighlight %}

该命令会在本地主机生成一个目录，与远程主机的版本库同名。如果要指定不同的目录名，可以将目录名作为git clone命令的第二个参数。
{% highlight ruby %}
$ git clone <版本库的网址> <本地目录名>
{% endhighlight %}

# 数字列表
1. 红
2. 黄
3. 蓝
3. 红
2. 黄
1. 蓝
{% highlight ruby %}
1. 红
2. 黄
3. 蓝
3. 红
2. 黄
1. 蓝
{% endhighlight %}

# 符号列表
+ 红
+ 黄
+ 蓝
{% highlight ruby %}
* 红
* 黄
* 蓝
+ 红
+ 黄
+ 蓝
- 红
- 黄
- 蓝
{% endhighlight %}

# 层级式列表
* 颜色  
    1. 红  
      + 紫红
      + 橘红
    2. 黄  
    3. 蓝  
      - 暗蓝
      - 亮蓝
      - 孔雀蓝
{% highlight ruby %}
* 颜色  
    1. 红    #[[4个空格/tab]]
      + 紫红 #[4个空格/tab + 2个以上空格]
      + 橘红
    2. 黄  
    3. 蓝  
      - 暗蓝
      - 亮蓝
      - 孔雀蓝
{% endhighlight %}

# 引用
>这是一段引用文本
>
>>嵌套引用文本
>>
>>> 嵌套引用文本
>>>
{% highlight ruby %}
> 这是一段引用文本
>> 嵌套引用文本
>>> 嵌套引用文本
{% endhighlight %}

# 水平分割线  
三个或更多*号为分割线

---  
***  
******
{% highlight ruby %}
***
---
******
{% endhighlight %}

# 代码段
使用一个反单引号可以生成一个行内代码块  
`行内代码块`

行起始为4个空格或1个Tab占位符则后续文本将被识别为代码段

	//代码段演示
	function aaa(){
		alert("一段javascript代码");
	}
{% highlight ruby %}
`行内代码块`
//代码段演示     # [4个空格或1个Tab]
function aaa(){  # [4个空格或1个Tab]
	alert("一段javascript代码"); # [4个空格或1个Tab]
}  # [4个空格或1个Tab]
{% endhighlight %}

# 图片
![演示无效图片](http:// "无效图片")
{% highlight ruby %}
![演示无效图片](http:// "无效图片")
{% endhighlight %}
![演示本地路径图片](./imgs/admin.png '本地路径图片')
{% highlight ruby %}
![演示本地路径图片](./imgs/admin.png '本地路径图片')
{% endhighlight %}
![演示远程路径图片](http://www.baidu.com/img/bdlogo.gif "远程路径图片")
{% highlight ruby %}
![演示远程路径图片](http://www.baidu.com/img/bdlogo.gif "远程路径图片")
{% endhighlight %}

![演示参考链接图片][002]
[002]:  http://img.blog.csdn.net/20140325110441140?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcHh6eQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast "参考链接图片"
{% highlight ruby %}
![演示参考链接图片][002]
[002]: http://img.blog.csdn.net/20140325110441140?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcHh6eQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast "参考链接图片"
{% endhighlight %}

# 链接
[内联样式链接](http://example.com "提示文本")
{% highlight ruby %}
[内联样式链接](http://example.com "提示文本")
{% endhighlight %}

[参考样式链接描述文本A][001]

[参考样式链接描述文本B][002]
[001]: http://about:blank "提示文本"
[002]: http://about:blank (提示文本)
{% highlight ruby %}
[参考样式链接描述文本A][001]
[参考样式链接描述文本B][002]
[001]: http://about:blank "提示文本"
[001]: http://about:blank (提示文本)
[002]: http://about:blank "提示文本"
[002]: http://about:blank (提示文本)
{% endhighlight %}

# 内联HTML标记
在MD文档里，你可以插入原生HTML标记，如：

<font color='red' style='font-size:20pt'>一段红色文本</font>
{% highlight ruby %}
<font color='red' style='font-size:20pt'>一段红色文本</font>
{% endhighlight %}

<table>
    <thead>
        <tr>
            <th>第一列</th>
            <th>第二列</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>第一行</td>
            <td>文本1</td>
        </tr>
        <tr>
            <td>第二行</td>
            <td>文本2</td>
        </tr>
    </tbody>
</table>
{% highlight ruby %}
<table>
    <thead>
        <tr>
            <th>第一列</th>
            <th>第二列</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>第一行</td>
            <td>文本1</td>
        </tr>
        <tr>
            <td>第二行</td>
            <td>文本2</td>
        </tr>
    </tbody>
</table>
{% endhighlight %}

## Python-Markdown常用扩展
# 目录 ([toc](http://pythonhosted.org/Markdown/extensions/toc.html))
* 功能  
此插件可以在文档内根据标题自动生成目录。
* 用法  
在文档起始处放置[toc]或者[TOC]标记即可让解析器自动生成目录。  

# 表格（[tables](http://pythonhosted.org/Markdown/extensions/tables.html)）
* 功能  
在文档里通过简单标记插入表格。
* 用法  
Markdown对表格的支持比较简陋，但可以满足一般的简单表格的展现需求。  
__注意：__ Markdown原生就支持内联HTML标记，因此，你也可以通过内联HTML的TABLE标记将表格插入到文档内。
* 例子

第一列 | 第二列 | 第三列 | 第四列  
----- | ----- | ----- | -----  
第一行 | 文本二 | 文本三 | 文本四  
第二行 | 文本二 | 文本三 | 文本四
{% highlight ruby %}
第一列 | 第二列 | 第三列 | 第四列  
----- | ----- | ----- | -----  
第一行 | 文本二 | 文本三 | 文本四  
第二行 | 文本二 | 文本三 | 文本四
{% endhighlight %}
第一列 | 第二列 | 第三列 | 第四列 | 第五列 | 第六列 | 第七列 | 第八列  
----- | ----- | ----- | ----- | ----- | ----- | ----- | -----  
第一行 | _文本二_ | 文本三 | **跨两列文本** | 文本六 | 文本七 | 文本八  
第二行 | 文本二 | 文本三 | 这是一段<br/>长文本 | 文本五 | <font color='green'>文本六</font> | 文本七 | 文本八
{% highlight ruby %}
第一列 | 第二列 | 第三列 | 第四列 | 第五列 | 第六列 | 第七列 | 第八列  
----- | ----- | ----- | ----- | ----- | ----- | ----- | -----  
第一行 | _文本二_ | 文本三 | **跨两列文本** | 文本六 | 文本七 | 文本八  
第二行 | 文本二 | 文本三 | 这是一段<br/>长文本 | 文本五 | <font color='green'>文本六</font> | 文本七 | 文本八
{% endhighlight %}
**注意：**由上面这个表格可知 tables 的表格不支持合并单元格！

# 公式（mathjax）
+ 功能  
在文档中通过 MathJax 标记插入公式。
+ 用法  
此插件基于开源公式显示引擎 MathJax (语法为类LaTex标记语言，最终输出W3C的MathML标记语言)
+ 例子  
可以支持像 π/2 $\pi$ 这样的内联公式。

markdown 可以支持像 \\(\frac{\pi}{2}\\) $\pi$ 这样的内联公式。

也可以是非内联的，如：

```markdown

F(ω)=12π−−√∫∞−∞f(t)e−iωtdt
\\[\int_0^1 f(t) \mathrm{d}t\\]

\\[\sum_j \gamma_j^2/d_j\\]
```
**注意：** MathJax 一般都需要访问网络，因此保证网络畅通才能正常显示。大多数流行的浏览器 都支持MathJax 。MathJax的语法 是类LaTex语法，标记繁多而复杂，不便于记忆。


# 指定标题ID（[headerid](http://pythonhosted.org/Markdown/extensions/header_id.html)）
- 功能  
在大型文档系统里，一般都有内容跳转功能，这种跳转通过链接实现，并且此功能必须基于设置在DOM元素上的id属性才能完成。这个扩展就能够指定某个标题的id属性。
- 用法  
你需要在要设置id属性的标题文本后加上#{#id-attr-value}，其中id-attr-value是id属性的值:
- 例子  
[跳转到“常用扩展”章节]()

## GitHub特性标记
注意：大多数MD解析器现在都支持全部或部分GitHub特性标记，但是在使用它们之前请还是要先确定一下是否支持！
# 代码段
与标准标记不同，GitHub风格的MD代码段是以两组三个连续的反单引号标记来标识，并且在第一组三个反单引号之后可以紧跟指定的代码语言名称，如：

javascript代码段定义：

```js
function aaa(){
    alert("一个javascript代码段");
}
```
{% highlight ruby %}
```js
function aaa(){
    alert("一个javascript代码段");
}
```
{% endhighlight %}
Python代码段定义：

```python
import random
class CardGame(object):
    """ a sample python class """
    NB_CARDS = 32
    def __init__(self, cards=5):
        self.cards = random.sample(range(self.NB_CARDS), 5)
        print 'ready to play'
```
{% highlight ruby %}
```python
import random
class CardGame(object):
    """ a sample python class """
    NB_CARDS = 32
    def __init__(self, cards=5):
        self.cards = random.sample(range(self.NB_CARDS), 5)
        print 'ready to play'
```
{% endhighlight %}

# 小图标
GitHub风格的MD标记可以让你将一些[预置的小图标](http://www.emoji-cheat-sheet.com/)加入到文档内。

:+1: :heart: :beer:  
:+1: :heart: :beer:  
    注意：所有这些图标资源都放在GitHub的服务器上而不是本地！解析器生成的HTML链接也是指向GitHub的服务器！
# 其他特性
更多GitHub特性Markdown语法请看[这里](https://help.github.com/articles/github-flavored-markdown/)

## 最佳实践
1. 尽量不要在MD文档内使用内联HTML标记，增强兼容性。
2. 将所有的参考式链接(包括图片参考链接)定义在MD文档的最后，便于维护。
3. 尽量使用标准语法，少用或不用不流行的扩展语法或特性语法。
4. 不要在一个MD文档内写非常多的内容，应该根据实际情况将内容分割到多个文件内，然后通过定义id属性值和链接来进行跳转

## Markdown编辑器
1. Sublime Text （win/linux/mac）[官网](http://www.sublimetext.com/)  
安装Markdown Preview插件后即可很好的支持Markdown文件的编辑和预览、转换。
![subtext](http://img.blog.csdn.net/20140325110648015?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcHh6eQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

	**特点：**
	- 基于Python-Markdown解析器。  
    - 语法高亮。
	- 丰富的文本编辑功能。
	- 自带简明语法手册(Cheat sheet)，能够给新手快捷指引。
	- 支持GitHub特性语法。
	- 支持HTML的导出和复制到剪贴板功能。
	- 支持常用的一些扩展（需要修改默认配置）。
	- 支持HTML模板。
	- 支持自定义样式
	
	**缺点：**
	- 不能导出除CSS以外的纯HTML标记代码段（需要手工复制）。
	- Sublime Text 2/3在linux下的中文输入存在问题（一个bug）。
	- 不支持实时预览，但可以先选择在浏览器中预览然后在文档修改后在浏览器中按F5刷新页面显示的方法来实现类似实时预览的功能。

2. reText（win/linux）[官网](http://sourceforge.net/projects/retext/?source=navbar)  
它是一个开源专业Markdown文档编辑器（同时也支持reStructuredText标记语言），一般在linux下使用。  
![reText](http://img.blog.csdn.net/20140325110715796?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcHh6eQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

	**特点：**
	- 语法高亮
    - 支持GitHub特性语法。
	- 生成的HTML文档中的样式为外部样式（即：使用）
	- 支持导出HTML、PDF和ODT格式文件。
	- 支持HTML模板。
	- 支持自定义样式。
	- 支持实时预览。
	
	**缺点：**
	- 在windows环境下难以安装（需要手动安装一大堆东西）。
	- 不能导出除CSS以外的纯HTML标记代码段（需要手工复制）。

3. MarkdownPad（win）[官网](http://markdownpad.com/)
是一款专业MD编辑器。
![MarkdownPad（win）](http://img.blog.csdn.net/20140325110740859?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcHh6eQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

	**特点：**
	- 语法高亮
    - 支持GitHub特性语法。
	- 支持导出HTML和PDF文件。
	- 支持自定义样式。
	- 离线支持GitHub风格MD标记
	- 支持实时预览。
	
	**缺点：**
	- 商业软件，没有购买之前有功能限制。
	- 没有HTML模板定制功能。

4. haroopad（win/linux/mac）[官网](http://pad.haroopress.com/user.html)  
韩国人开发的一款新一代MD编辑器，很有潜力的MD编辑器。  
![haroopad](http://img.blog.csdn.net/20140325110807437?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcHh6eQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

	**特点：**
	- 语法高亮
	- 基于Chromium开发。
    - 支持GitHub特性语法。
	- 生成的HTML文档中的样式为外部样式（即：使用）。
	- 支持导出除CSS以外的纯HTML标记代码段。
	- 具有简洁的MD语法导航边栏。
	- 支持导出HTML文件。
	- **支持将解析出的内容作为邮件发送。**
	- 支持自定义样式。
	- 支持实时预览。
	- **支持编辑区与预览区域的同步滚动。**
	- **支持语法检测，在有问题的标记文本下面将显示红色波浪线。**
	- **支持VIM编辑模式。**
	- **与社交平台集成。**
	
	**缺点：**
	- 当前还处在Beta测试阶段，功能不够稳定。
	- 没有HTML模板定制功能。

## 附录
# Markdown解析器各大实现
* [Python-Markdown](http://pythonhosted.org/Markdown/)
基于Python的MD解析器
* [Python-Markdown2](https://github.com/trentm/python-markdown2)
基于Python的MD解析器  
注意：Python-Markdown2并不是Python-Markdown的后续版本！
* multimarkdown  
引入更多标记特性和输出选项的改进版Markdown解析器
* kramdown  
基于Ruby的MD解析器
* PHP Markdown  
基于php的MD解析器
* parsedown  
基于php的MD解析器
* pandoc
命令行文档转换神器！自带MD解析器

# Python-Markdown的Extra扩展
Extra扩展是Python-Markdown标准扩展集

* abbr -- Abbreviations
* attr_list -- Attribute Lists
* def_list -- Definition Lists
* fenced_code -- Fenced Code Blocks
* footnotes -- Footnotes
* tables -- Tables
* smart_strong -- Smart Strong

# Python-Markdown的其他扩展

* code-hilite -- CodeHilite
* html-tidy -- HTML Tidy
* header-id -- HeaderId
* meta_data -- Meta-Data
* nl2br -- New Line to Break
* rss -- RSS
* sane_lists -- Sane Lists
* toc -- Table of Contents
* wikilinks -- WikiLinks

{% highlight ruby %}
文档信息
--------------
* 版权声明：自由转载-非商用
* 转载: []
{% endhighlight %}

[Markdown简明教程](http://www.ruanyifeng.com/blog/2014/06/git_remote.html)

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
