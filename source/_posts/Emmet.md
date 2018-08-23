---
title: Emmet的用法
date: 2018-08-23 09:54:33
tags: Emmet
categories: 前端
---
# 引入

用类似css选择器的写法，生成HTML语句。

比如：

```Emmet
#page>div.logo+ul#navigation>li*5>a{Item $}
```

在支持的编辑器里按tab即可生成：

```html
<div id="page">
    <div class="logo"></div>
    <ul id="navigation">
        <li><a href="">Item 1</a></li>
        <li><a href="">Item 2</a></li>
        <li><a href="">Item 3</a></li>
        <li><a href="">Item 4</a></li>
        <li><a href="">Item 5</a></li>
    </ul>
</div>
```

# 标识符

## 1. 子节点 >

```Emmet
div>ul>li
```

```html
<div>
    <ul>
        <li></li>
    </ul>
</div>
```

## 2. 兄弟节点 +

```Emmet
div+p+bq
```

```html
<div></div>
<p></p>
<blockquote></blockquote>
```

## 3. 上浮 ^

```Emmet
div+div>p>span+em^bq 
```

```html
<div></div>
<div>
    <p><span></span><em></em></p>
    <blockquote></blockquote>
</div>
```

```Emmet
div+div>p>span+em^^^bq
```

```html
<div></div>
<div>
    <p><span></span><em></em></p>
</div>
<blockquote></blockquote>
```

## 4. 倍数 *

```Emmet
div+p+bq
```

```html
<div></div>
<p></p>
<blockquote></blockquote>
```

## 5. 分组 ()

```Emmet
div>(header>ul>li*2>a)+footer>p
```

```html
<div>
    <header>
        <ul>
            <li><a href=""></a></li>
            <li><a href=""></a></li>
        </ul>
    </header>
    <footer>
        <p></p>
    </footer>
</div>
```

```Emmet
(div>dl>(dt+dd)*3)+footer>p
```

```html
<div>
    <dl>
        <dt></dt>
        <dd></dd>
        <dt></dt>
        <dd></dd>
        <dt></dt>
        <dd></dd>
    </dl>
</div>
<footer>
    <p></p>
</footer>
```

## 6. id \#

```Emmet
div#header
```

```html
<div id="header"></div>
```

## 7. class .

```Emmet
div.page
```

```html
<div class="page"></div>
```

```Emmet
div#header+div.page+div#footer.class1.class2.class3
```

```html
<div id="header"></div>
<div class="page"></div>
<div id="footer" class="class1 class2 class3"></div>
```

## 8. 自定义属性 []

```Emmet
td[title="Hello world!" colspan=3]
```

```html
<td title="Hello world!" colspan="3"></td>
```

## 9. 自增长 $

```Emmet
ul>li.item$*5
```

```html
<ul>
    <li class="item1"></li>
    <li class="item2"></li>
    <li class="item3"></li>
    <li class="item4"></li>
    <li class="item5"></li>
</ul>
```

```Emmet
ul>li.item$$$*5
```

```html
<ul>
    <li class="item001"></li>
    <li class="item002"></li>
    <li class="item003"></li>
    <li class="item004"></li>
    <li class="item005"></li>
</ul>
```

```Emmet
ul>li.item$@-*5
```

```html
<ul>
    <li class="item5"></li>
    <li class="item4"></li>
    <li class="item3"></li>
    <li class="item2"></li>
    <li class="item1"></li>
</ul>
```

```Emmet
ul>li.item$@3*5
```

```html
<ul>
    <li class="item3"></li>
    <li class="item4"></li>
    <li class="item5"></li>
    <li class="item6"></li>
    <li class="item7"></li>
</ul>
```

```Emmet
ul>li.item$@-3*5
```

```html
<ul>
    <li class="item7"></li>
    <li class="item6"></li>
    <li class="item5"></li>
    <li class="item4"></li>
    <li class="item3"></li>
</ul>
```

## 10. 文本 {}

```Emmet
a{Click me}
```

```html
<a href="">Click me</a>
```

```Emmet
a{click}+b{here}
```

```html
<a href="">click</a><b>here</b>
```

```Emmet
a>{click}+b{here}
```

```html
<a href="">click<b>here</b></a>
```

```Emmet
p>{Click }+a{here}+{ to continue}
```

```html
<p>Click <a href="">here</a> to continue</p>
```

```Emmet
p{Click }+a{here}+{ to continue}
```

```html
<p>Click </p>
<a href="">here</a> to continue
```

## 注意
不要在Emmet中添加空格，比如：
```Emmet
(header > ul.nav > li*5) + footer
```
这样会无法识别。