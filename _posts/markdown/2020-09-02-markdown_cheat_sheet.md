---
layout: post
title: [转]markdown常用语法
categories: [markdown]
description: markdown常用语法
keywords: markdown,语法
---

## 原文
[buaawht markdown常用语法](https://buaawht.github.io/wiki/markdown)

加入了自己的修改

**目录**

* TOC
{:toc}


### 超链接

```
[靠谱-ing](https://mazhuang.org)

<https://mazhuang.org>
```

[靠谱-ing](https://mazhuang.org)  

<https://mazhuang.org>

### 列表

```
1. 有序列表项 1

2. 有序列表项 2

3. 有序列表项 3
```

1. 有序列表项 1

2. 有序列表项 2

3. 有序列表项 3

```
* 无序列表项 1

* 无序列表项 2

* 无序列表项 3
```

* 无序列表项 1

* 无序列表项 2

* 无序列表项 3

```
- [x] 任务列表 1
- [ ] 任务列表 2
```

- [x] 任务列表 1
- [ ] 任务列表 2

### 强调

```
~~删除线~~

**加黑**

*斜体*
```

~~删除线~~

**加黑**

*斜体*

### 标题

```
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
```

Tips: `#` 与标题中间要加空格。

### 表格

```
| HEADER1 | HEADER2 | HEADER3 | HEADER4 |
| ------- | :------ | :-----: | ------: |
| content | content | content | content |
```

| HEADER1 | HEADER2 | HEADER3 | HEADER4 |
| ------- | :------ | :-----: | ------: |
| content | content | content | content |

1. :----- 表示左对齐
2. :----: 表示中对齐
3. -----: 表示右对齐

### 代码块

```python
print 'Hello, World!'
```

1. list item1

2. list item2

   ```python
   print 'hello'
   ```

### 图片

```
![本站favicon](/favicon.ico)
```

![本站favicon](/favicon.ico)

### 锚点

```
* [目录](#目录)
```

* [目录](#目录)

### Emoji

:camel:
:blush:
:smile:

### Footnotes

This is a text with footnote[^1].

### mermaid

<div class="mermaid">
sequenceDiagram
    Alice-->>John: Hello John, how are you?
    John-->>Alice: Great!
</div>

### sequence

```sequence
Andrew->China: Says Hello
Note right of China: China thinks\nabout it
China-->Andrew: How are you?
Andrew->>China: I am good thanks!
```

### flowchart

```flow
st=>start: Start
e=>end
op1=>operation: My Operation
sub1=>subroutine: My Subroutine
cond=>condition: Yes
or No?
io=>inputoutput: catch something...

st->op1->cond
cond(yes)->io->e
cond(no)->sub1(right)->op1
```

### mathjax

When $$(a \ne 0)$$, there are two solutions to $$(ax^2 + bx + c = 0)$$ and they are

$$x = {-b \pm \sqrt{b^2-4ac} \over 2a}.$$

[^1]: Here is the footnote 1 definition.

[GitHub博客markdown文章插入LateX公式](https://geekstar.tech/2020/07/17/github_page_add_latex_formula/)


### 修改颜色
这里其实使用的是html语法

<font color='red'>红色的字体</font>

```
<font color='red'>红色的字体</font>
```