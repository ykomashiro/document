# Markdown语法总结

## 标题

用 Markdown 书写时，只需要在文本前面加上『# 』即可创建一级标题。同理，创建二级标题、三级标题等只需要增加『# 』个数即可，Markdown 共支持六级标题。

## 引用

Markdown 标记区块引用和 email 中用『>』的引用方式类似，只需要在整个段落的第一行最前面加上『>』

区块引用可以嵌套，只要根据层次加上不同数量的『>』

引用的区块内也可以使用其他的 Markdown 语法，包括标题、列表、代码区块等

## 列表

列表项目标记通常放在最左边，项目标记后面要接一个字符的空格。

无序列表：使用星号、加号或是减号作为列表标记
```markdown
- Red
- Green
- Blue
```
有序列表：使用数字接着一个英文句点
```markdown
1. Red
2. Green
3. Blue
```
代办列表: 表示列表是否勾选状态（注意：[ ] 前后都要有空格）
```markdown
- [ ] 不勾选
- [x] 勾选
```

## 代码

只要把你的代码块包裹在"`"之间，就不需要通过无休止的缩进来标记代码块了。 在围栏式代码块中，可以指定一个可选的语言标识符，然后我们就可以为它启用语法着色了。

## 强调
在Markdown中，可以使用 * 和 _ 来表示斜体和加粗。
**斜体**
```markdown
*Coding，让开发更简单*
_Coding，让开发更简单_
```
__加粗__
```markdown
**Coding，让开发更简单**
__Coding，让开发更简单__
```

## 自动链接
方括号显示说明，圆括号内显示网址， Markdown 会自动把它转成链接，例如：
```markdown
[超强大的云开发平台Coding](http://coding.net)
```

## 表格

在 Markdown 中，可以制作表格，并且可以让表格两边内容对齐，中间内容居中，例如：
```markdown
First Header | Second Header | Third Header
:----------- | :-----------: | -----------:
Content Cell | Content Cell  | Content Cell
Content Cell | Content Cell  | Content Cell
```
First Header | Second Header | Third Header
:----------- | :-----------: | -----------:
Left         | Center        | Right
Left         | Center        | Right

## 分割线

在 Markdown 中，可以使用 3 个以上『-』符号制作分割线，例如：
```markdown
这是分隔线上部分内容
----
这是分隔线上部分内容
```

这是分隔线上部分内容

---
这是分隔线上部分内容

## 图片

Markdown 使用了类似链接的语法来插入图片, 包含两种形式: 内联和引用.

内联图片语法如下:
```markdown
![Alt text](/path/to/img.jpg)
或
![Alt text](/path/to/img.jpg "Optional title")
```
![tupian](F:/Script/blhx.png)
引用图片语法如下:
```markdown
![Alt text][id]
```
『id』 是图片引用的名称. 图片引用使用链接定义的相同语法:
```markdown
[id]: url/to/image "Optional title attribute"
```

## 流程图

Markdown 编辑器已支持绘制流程图、时序图和甘特图。通过 mermaid 实现图形的插入，点击查看[更多语法详情](https://mermaidjs.github.io/)。

## 数学公式
Markdown可以使用`latex`来编写数学公式。
\[e=mc\]



