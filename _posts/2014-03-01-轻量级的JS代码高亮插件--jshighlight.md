---
layout: post
title: 轻量级的JS代码高亮插件-jshighlight
tags: [plugins, work, javascript]
---


![jshighlight](/images/content/plugins/jshighlight/jshighlight.jpg)

`jshighlight`-一款基于javascript的轻量级的代码着色插件，这个插件使用比较简单，而且代码比较少。
虽然原生只支持html、css、javascript，但是它也可以被扩展以支持其他的语言，下面会讲到怎么去扩展它
，本博客使用以查看这篇文章中的代码，下面简要介绍一下她的一些信息

## 插件特点

* 真正轻量级，JS代码压缩后3K左右；

* 调用方便，引入jshighlight核心js文件即可；

* 不依赖于任何其他库；

* 原生支持HTML、CSS、Javascript；

* 支持其他语言的轻松扩展；

* 显示行号，直接复制代码不会复制行号；

* 提供四套主题可选，默认使用`Monokai`样式主题；

## 使用步骤

1.在`<head>`中引入相应的样式文件：

~~~html
<!--默认样式-->
<link href="../theme/jshighlight-default.css" rel="stylesheet" />
~~~

2.在`</body>`前中引入相应js文件：

~~~html
<!--核心js文件-->
<script src="../js/jshighlight.core-v1.0.1.min.js"></script>
~~~

3.在需要着色的pre标签中加入`data-language`属性，取值为：`javascript|html|css`，扩展后可以设置其他的值；

## 如何扩展

1.在`<body>`中引入相应js文件：

~~~html
<script src="../js/jshighlight.core-v1.0.0.min.js"></script>
~~~

2.自定义需要着色的语言所需要的样式，例如：

~~~css
.php-com{
    color: #CCC;
}
.php-mrk{
    color: red;
    font-weight: bold;
}
.php-bol{
    color: #F92665;
    font-style: italic;
}
.php-var{
    color: #A6E22E;
}
/*.......*/
/* 也可以使用默认的样式，传入默认样式类名即可，
 * 样式名称可以自由使用，比如注释对应的样式也可以用.key
 * 默认样式如下：
 */
.com{ color:#75715E } /*普通注释*/
.doc{ color:#48BEEF } /*文档注释*/
.str{ color:#E6DB74 } /*字符串*/
.key{ color:#48BEEF; font-weight: bold; font-style: italic } /*关键字*/
.obj{ color:#AE81FF; font-weight:bold } /*内置对象、函数*/
.num{ color:#F92672 } /*数字*/
.ope{ color:#FD971F } /*操作符*/
.bol{ color:#FF5600; font-style: italic } /*布尔值*/

.mrk{ color:#F92665 } /*html标签*/
.attr{ color:#A6E22E } /*属性名称*/
.val{ color:#E6DB74 } /*属性值*/
~~~

3.定义提取需要着色的内容的正则，比如：

~~~javascript
reg = {
    'com' : /(\/\*[\s\S]*?\*\/|\/\/.*|&lt;\!--[\s\S]*?--&gt;)/, //普通注释
    'mrk' : /(&lt;\?php|\?&gt;)/, //标签
    'str' : /('(?:(?:\\'|[^'\r\n])*?)'|"(?:(?:\\"|[^"\r\n])*?)")/, //字符串
}
~~~

4.调用`JSHL`的`extendLanguage`方法：

~~~javascript
JSHL.extendLanguage('php',{
   /*
    * 每个分组对应的样式类名
    * 比如：'com'中有一个分组，'mrk'中有一个分组，'key'中有两个分组，
    * 那么： com中的分组对应'php-com','mrk'中的分组对应
    * 'php-mrk','key'中的第一个分组对应'str',第二个对应'key'，以此类推；
    */
   cls : ['php-com','php-mrk','str','key','php-var','obj','num','php-bol','ope'],
   reg : {
        'com' : /(\/\*[\s\S]*?\*\/|\/\/.*|<\!--[\s\S]*?-->)/,  //普通注释
        'mrk' : /(<\?php|\?>)/, //标签
        'str' : /('(?:(?:\\'|[^'\r\n])*?)'|"(?:(?:\\"|[^"\r\n])*?)")/, //字符串
        'key' : /(?:[^$_@a-zA-Z0-9])?(and|or|...|throw)(?![$_@a-zA-Z0-9])/, //关键字
        'var' : /(\$[\w][\w\d]*)/, //变量名
        'obj' : /(?:[^$_@A-Za-z0-9])?(echo|...|date)(?:[^$_@A-Za-z0-9])/, //内置函数(部分)
        'num' : /\b(\d+(?:\.\d+)?(?:[Ee][-+]?(?:\d)+)?)\b/,  //数字
        'bol' : /(?:[^$_@A-Za-z0-9])?(true|false)(?:[^$_@A-Za-z0-9])/, //布尔值
        'ope' : /(==|=|===|\+|-|\+=|-=|\*=|\\=|%=|<|<=|>|>=|\.)/  //操作符
    },
    //如果这个语言是包含在html中的设置下列属性
    wrapper: 'html',
    content : {
        lang : 'php', // 语言名称，在于pre标签的data-language一致
        wrapper : /()/g // 需要着色的代码被包裹的形式
    }
})
~~~

## 实际效果

### Default Theme--Monokai
![Default theme](/images/content/plugins/jshighlight/default.png)

### iPlastic Theme
![iPlastic theme](/images/content/plugins/jshighlight/iPlastic.png)

### Eiffel Theme
![Eiffel theme](/images/content/plugins/jshighlight/Eiffel.png)

### Blackboard Theme
![Blackboard theme](/images/content/plugins/jshighlight/Blackboard.png)

### Source code

<https://github.com/zdying/jshighlight>
