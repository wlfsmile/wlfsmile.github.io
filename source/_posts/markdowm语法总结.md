---
title: markdowm语法总结
date: 2017-01-14 14:23:59
tags: markdown
categories: 前端
---
### 对一些简单markdown的语法理解
![markdown](/img/markdown.jpg)
##### 这也是我第一次用markdown语法写的文章，还是有很多不足
<!--more-->
##### 【前提概要】
对于一个markdown小白来总结这篇文章，我理解的最深刻的就是，HTML与markdown语法之间的联系。简而言之，HTML不能使用markdown的语法，但是markdown中支持在HTML中的语法使用。所有我一开始用的HTML语法来写的github的README.md文档达到了我想要的效果 ，比如\< p>或者\< img>标签，也可以用来作为markdown中的行段落和插入图片。  

##### 【段落和换行】
要写一篇文章的话，对于段落感跟换行是必须的，可以用HTML中的\< br>标签进行换行， 但在markdown中，也可以用两个或两个以上的空格加回车键，就可以换好行了。  

##### 【标题】
Markdown 支持两种标题的语法，类 Setext 和类 atx 形式。  
类 Setext 形式是用底线的形式，利用 = （最高阶标题）和 - （第二阶标题）。  
相比于类Setext形式，我更喜欢用类atx形式，相比也比较简单，直接在标题前面加上#即可，几号标题加几个#即可（1到6个）。  

##### 【区块引用 Blockquotes】
在文字开头添加“>”表示区块引用（块注释）  

##### 【斜体和粗体】
在需要斜体的文字两端用"\*"或者"\_"夹起来，而如要粗体，则用两个"\*"或者"\_"夹起来。  

##### 【无序与有序列表】
在文字开头添加(\*, +, and -)实现无序列表。但是要注意在(\*, +, and -)和文字之间需要添加空格。(一个文档中最好只是用一种无序列表的表示方式)，而有序列表在使用数字后面加上句号和空格即可。  

##### 【链接】
实现链接一共有两种基本方式：内联和引用方式。  
内联：在链接的文字外加上\[\],而在\[\]外加上(里面是链接的地址)，如：这篇文章主要来自[markdown语法](http://www.appinn.com/markdown/)。  
引用：这篇文章主要来自\[markdown语法\]\[1\]，\[第二个\]\[2\]  
\[1\]: http://www.appinn.com/markdown/  "markdown"  
\[2\]: http://www.appinn.com/markdown/  "markdown"  
其中"markdown"表示鼠标移到链接的文字上去，会显示出来的内容，如：  
引用：这篇文章主要来自[markdown语法][1]，[第二个][2]  
[1]: http://www.appinn.com/markdown/  "markdown"  
[2]: http://www.appinn.com/markdown/  "markdown"  

##### 【插入图片】
插入图片的方式跟链接的方式类似。  
内联方式：\!\[alt text\]\(/path/to/img.jpg "Title"\)
引用方式：
\!\[alt text\]\[id\]  
\[id\]: /path/to/img.jpg "Title"  

##### 【代码框】
有两种方式，第一种是在一些比较简单的代码中，可以直接使用\` < blockquote>\`来实现。第二种是大片文字需要实现代码框。使用Tab或四个空格。  

##### 【脚注】
hello\[^hello\]
\[^hello\]: hi  
效果为：  
hello[^hello]
[^hello]: hi  

##### 【下划线】
在空白行下方添加三条“-”横线。




