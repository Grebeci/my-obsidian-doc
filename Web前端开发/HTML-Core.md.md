HTML Element



Element （元素）

Attribute（属性）

Tag （标签）



![](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Introduction_to_HTML/Getting_started/grumpy-cat-small.png)



元素

嵌套元素（Nesting）

块级元素，内联元素

空元素（Void）

> 中文教程环境下经常说单标签，意思是没有成对出现的标签。



属性

![含有‘class="editor-note"’属性的段落标签](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Introduction_to_HTML/Getting_started/grumpy-cat-attribute-small.png)



布尔属性

布尔属性只能有一个值，这个值一般和属性名相同。也可以只写属性名没有属性值



HTML 结构

```html
<!doctype html>
<html lang="zh-CN">
  <head>
    <meta charset="utf-8" />
    <title>我的测试站点</title>
  </head>
  <body>
    <p>这是我的页面</p>
  </body>
</html>
```

1. `<!DOCTYPE html>`: 声明文档类型。这是历史遗留标签，这种写法是最简的。
2. `<html></html>` ： 根元素，包含页面的所有内容。
3. `<meta charset="utf-8">` ：这个元素代表了不能由其他 HTML 元相关元素表示的元数据， follow 这个即可。
4. `<title></title> ` ：页面标题，浏览器标签页的内容，当页面被加入书签时，这会是默认名称。
5. `<body></body>`：页面内容。



“空白折叠”（Whitespace Collapsing）

在HTML中，当渲染页面时，连续的空白字符（如多个空格或换行符）通常会被浏览器解释为单个空格。

```
无论你在 HTML 元素的内容中使用多少空格（包括一个或多个空白字符或换行），当渲染这些代码的时候，HTML 解释器会将连续出现的空白字符减少为一个单独的空格符。

这句话来自MDN，这个特定具体是什么名字，有啥规范的术语吗?



这个HTML中的特性称为“空白折叠”（Whitespace Collapsing）。空白折叠是HTML和CSS中的一个标准行为，它涉及如何处理元素内容中的空白字符（包括空格、制表符和换行符）。

在HTML中，当渲染页面时，连续的空白字符（如多个空格或换行符）通常会被浏览器解释为单个空格。这种行为基于HTML的内容模型，该模型将大多数空白视为可折叠的，从而简化了文档的视觉呈现。

**关于空白折叠的重要点包括：**
- 在HTML文档中，多个连续的空白字符（包括空格、制表符、换行符等）通常会被浏览器渲染为一个单一的空格。
- 这种行为有助于简化HTML代码的编写，因为你不需要担心代码中的额外空白会影响最终的页面布局。
- 但是，在`<pre>`标签内的文本不遵循空白折叠规则，这意味着在`<pre>`元素内部的所有空白字符都会被保留并按原样显示。

空白折叠是CSS规范中定义的一部分，由`white-space`属性控制。这个属性决定了给定元素的空白字符是如何处理的，允许你在需要时改变默认的折叠行为。例如，`white-space: pre;` 属性设置可以保持空白符的原样显示，而`white-space: normal;` 则应用标准的空白折叠行为。
```





实体（字符）引用

类似于转义字符，这些特殊字符需要重新编码，每个字符引用以符号 & 开始，以分号（;）结束。



注释

```html
<!-- comment -->
```















#### HTNL 元素学习日志

##### `<p>`

【英文缩写】： Paragraph

【语义】：

##### `<em>`

【英文缩写】： emphasis

【语义】：

##### `<strong>`

【英文缩写】： strong

【语义】：





