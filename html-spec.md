# HTML编码规范 (1.1)


## 简介

该文档主要的设计目标是HTML和CSS开发风格保持一致，容易被理解和被维护。

### 要求

在本文档中，使用的关键字会以中文+括号包含的关键字英文表示：必须（MUST）。关键字"MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL"被定义在rfc2119中。


## 通用

### 编码

使用UTF-8编码。HTML、CSS、模板等文件 *不允许(MUST NOT)* 包含BOM信息。

## HTML

### 缩进

*必须(MUST)* 采用**4个空格**为一次缩进， *不得(MUST NOT)* 采用TAB作为缩进。

### 命名

+ id命名 *必须(MUST)* 采用单词全字母小写，单词间以中划线分隔的方式，如`this-is-a-id`。
+ class命名 *必须(MUST)* 采用单词全字母小写，单词间以中划线分隔的方式，如`this-is-a-class`。
+ name命名产品线可自己定义，但产品线内 *必须(MUST)* 保持一致。

### DOCTYPE

HTML文档 *必须(MUST)* 以DOCTYPE声明起始。 *必须(MUST)* 使用HTML5标准的DOCTYPE声明， *不得(MUST NOT)* 使用任何html4以及xhtml的DOCTYPE声明。

```html
<!DOCTYPE html>
```

### charset声明

页面必须（MUST）声明编码方式之后。HTML文档必须（MUST）在head区域进行charset声明。

```html
<meta charset="utf-8" />
```

### title

页面 *必须(MUST)* 包含title标签声明标题。title *必须(MUST)* 位于head标签中，并置于charset声明之后。

```html
<head>
    <meta charset="utf-8" />
    <title>页面标题</title>
</head>
```

### 样式定义与引入

样式定义，包含link标签外部引入与style标签定义的页面内样式， *必须(MUST)* 放在head标签中。

引入外部样式 *必须(MUST)* 使用link标签。 *不允许(MUST NOT)* 使用`@import`引入外部样式，构建时会被合并内容的引用除外。

后缀为".css"的外部样式资源引入，link标签 *应当不(SHOULD NOT)* 包含`type`属性。

```html
<link rel="stylesheet" href="yourcss.css" />
```


### javascript引入

*尽量(SHOULD)* 将外部引入的js资源置于body标签结束之前。

对于可延迟执行的`<script>`标签， *建议(RECOMMENDED)* 加上`defer="defer"`属性。

对于执行顺序无关的`<script>`标签， *建议(RECOMMENDED)* 加上`async="async"`属性。

后缀为".js"的外部脚本资源引入，script标签 *应当不(SHOULD NOT)* 包含type属性。

```html
<script src="yourscript.js"></script>
```


### 字符实体

除以下字符名， *不要(MUST NOT)* 使用字符实体。

- `&` - `&amp;`
- `<` - `&lt;`
- `>` - `&gt;`
- `"` - `&quot;`

### 标签名和属性名

标签名和属性名 *必须(MUST)* 使用小写字母。

### id属性

页面编写者 *必须(MUST)* 保持元素id的唯一性。

### 标签自闭合

对于允许不自闭合的标签， *不得(MUST NOT)* 添加闭合标签， *必须(MUST)* 使用自闭合形式，且`/`前 *必须(MUST)* 包含一个空格。常见标签有input、br、img、hr等。

```html
<input type="text" name="title" />
```

### 省略标签闭合

*不可(MUST NOT)* 省略闭合的标签。常见标签有li、dt、dd、html、tr、td、th、thead、tbody、tfoot等。

```html
<li>listitem</li>
```

### 过时标签

*避免(MUST NOT)* 使用过时的标签。常见的有basefont,center,dir,font,isindex,menu,strike。


### boolean属性

对于boolean类型的属性， *必须(MUST)* 加上与属性名相同的属性值。

```html
<input type="checkbox" checked="checked" />
```

### 标签语义化

*尽量采用(REQUIRED)* 语义化标签。常见标签语义如下：


+ p - 段落
+ h1,h2,h3,h4,h5,h6 - 层级标题
+ strong,em - 强调
+ ins - 插入
+ del - 删除
+ abbr - 缩写
+ code - 代码标识
+ cite - 引述来源作品的标题
+ q - 引用
+ blockquote - 一段或长篇引用
+ ul - 无序列表
+ ol - 有序列表
+ dl,dt,dd - 定义列表


### 标签嵌套规则

编写的html *必须(MUST)* 符合标签嵌套规则。如：div标签不得置于p标签中，tbody标签必须置于table标签中。

标签嵌套规则请参考[HTML5 DTD](http://www.cs.tut.fi/~jkorpela/html5.dtd)，注意ELEMENT定义部分。


### 样式控制

除控制逻辑相关外，比如显示/隐藏， *不允许(MUST NOT)* 在html中编写style=""行内样式。

*不得(MUST NOT)* 使用bgcolor和background属性定义背景。

### 事件属性

*不得(MUST NOT)* 在html中编写onclick="xxx"的内联事件绑定，保持html纯洁，不涉及逻辑控制。

### img

img标签 *必须(MUST)* 包含src、alt属性， *尽量(SHOULD)* 包含width、height属性，避免引起页面跳跃。

装饰性图片 *不得(MUST NOT)* 使用img标签，应使用css定义background。

```html
<img src="imgurl" width="80" height="60" alt="alt info" />
```

### table

tr标签 *必须(MUST)* 位于tbody、thead、tfoot标签中， *不得(MUST NOT)* 直接置于table标签中。

### a

*不得(MUST NOT)* 为鼠标手样式使用href="javascript:void(0)"的a标签。应采用CSS的cursor:pointer。


### 表单

*允许(SHOULD)* 使用table令表单左右对齐。

*尽量(SHALL)* 为表单绑定label标签。
