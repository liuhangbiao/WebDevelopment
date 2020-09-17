## HTML规范 - 代码格式

### 说明文案的注释方法

采用类似标签闭合的写法，与HTML统一格式；注释文案两头空格，与CSS注释统一格式。

* 开始注释：<!-- 注释文案 -->（文案两头空格）。
* 结束注释：<!-- /注释文案 -->（文案前加“/”符号，类似标签的闭合）。
* 允许只有开始注释！

```html
<!-- 头部 -->
<div class="g-hd">
    <!-- LOGO -->
    <h1 class="m-logo"><a href="#">LOGO</a></h1>
    <!-- /LOGO -->
    <!-- 导航 -->
    <ul class="m-nav">
        <li><a href="#">导航一</a></li>
        <li><a href="#">导航二</a></li>
        <!-- 更多导航项 -->
    </ul>
    <!-- /导航 -->
</div>
<!-- /头部 -->
```

### 代码本身的注释方法

单行代码的注释也保持同行，两端空格；多行代码的注释起始和结尾都另起一行并左缩进对齐。

```html
<!-- <h1 class="m-logo"><a href="#">LOGO</a></h1> -->
<!--
<ul class="m-nav">
    <li><a href="#">NAV1</a></li>
    <li><a href="#">NAV2</a></li>
</ul>
-->
```

### HTML注释在IE6中的BUG

* 如果两个浮动元素之间存在注释，那么可能导致布局错位或文字的BUG。
* 所以，这种情况下，我们通常将注释去掉，或者索性采用模板语言（ftl、vm）的注释。

### 严格的嵌套

* 尽可能以最严格的xhtml strict标准来嵌套，比如内联元素不能包含块级元素等等。
* 正确闭合标签且必须闭合。

### 严格的属性

* 属性和值全部小写，每个属性都必须有一个值，每个值必须加双引号。
* 没有值的属性必须使用自己的名称做为值（checked、disabled、readonly、selected等等）。
* 可以省略style标签和script标签的type属性。


### 常用的标签

|标签|语义|嵌套常见错误|常用属性（加粗的为不可缺少的或建议的）|
|:-|:-|:-|:-|
|`<a></a>`|超链接/锚|a不可嵌套a|href,name,title,rel,target|
|`<br />`|换行| | |
|`<button></button>`|按钮|不可嵌套表单元素|type,disabled|
|`<dd></dd>`|定义列表中的定义（描述内容）|只能以dl为父容器，对应一个dt|     |
|`<del></del>`|文本删除| | |
|`<div></div>`|块级容器| | |
|`<dl></dl>`|定义列表|只能嵌套dt和dd|  |
|`<dt></dt>`|定义列表中的定义术语|只能以dl为父容器，对应多个dd|     |
|`<em></em>`|强调文本| | |
|`<form></form>`|表单|   |action,target,method,name|
|`<h1></h1>`|标题 |从h1到h6，不可嵌套块级元素|    |
|`<iframe></iframe>`|内嵌一个网页| |frameborder,width,height,src,scrolling,name|
|`<img />`| 图像  | |alt,src,width,height|
|`<input />`|各种表单控件| |type,name,value,checked,disabled,maxlength,readonly,accesskey|
|`<label></label>`| 标签为input元素定义标注| |for|
|`<li></li>`|列表项|只能以ul或ol为父容器|   |
|`<link />`|引用样式或icon|不可嵌套任何元素|type,rel,href|
|`<meta />`|文档信息|只用于head|content,http-equiv,name|
|`<ol></ol>`|有序列表|只能嵌套li| |
|`<option></option>`|select中的一个选项|仅用于select|value,selected,disabled|
|`<p></p>`|段落|不能嵌套块级元素| |
|`<script></script>`|引用脚本|不可嵌套任何元素|type,src|
|`<select></select>`|列表框或下拉框|只能嵌套option或optgroup|name,disabled,multiple|
|`<span></span>`|内联容器|| |
|`<strong></strong>`|强调文本|| |
|`<style></style>`|引用样式|不可嵌套任何元素|type,media|
|`<sub></sub>`| 下标  | | |
|`<sup></sup>`| 上标  | | |
|`<table></table>`| 表格  |只可嵌套表格元素   |width,align,background,cellpadding,cellspacing,summary,border|
|`<tbody></tbody>`| 表格主体|只用于table|   |
|`<td></td>`|表格中的单元格|只用于tr|colspan,rowspan|
|`<textarea></textarea>`|多行文本输入控件| |name,accesskey,disabled,readonly,rows,cols|
|`<tfoot></tfoot>`| 表格表尾|只用于table| |
|`<th></th>`|表格中的标题单元格|只用于tr|colspan,rowspan|
|`<thead></thead>`|表格表头|只用于table|    |
|`<title></title>`|文档标题|只用于head| |
|`<tr></tr>`|表格行|嵌套于table或thead、tbody、tfoot| |
|`<ul></ul>`|无序列表|只能嵌套li| |