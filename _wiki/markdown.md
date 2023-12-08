---
layout: wiki
title: Markdown
cate1: Copywriting
cate2: 
description: Markdown 常用语法示例。
keywords: Markdown
mermaid: true
sequence: true
flow: true
mathjax: true
mindmap: true
---

**目录**

* TOC
{:toc}

### 超链接

```
[靠谱-ing](https://telegeam.github.io)

<https://telegeam.github.io>
```

[靠谱-ing](https://telegeam.github.io)  

<https://telegeam.github.io>

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

### Inline Attribute

Span Inline Attribute 详情参考 <https://kramdown.gettalong.org/syntax.html#span-ials>

Block Inline Attribute 详情参考 <https://kramdown.gettalong.org/syntax.html#block-ials>

给块/元素添加 class、id、内嵌样式等：

```
![本站favicon](/favicon.ico){:.center}

Hello, *world*{:#world} 

Hello, *world*{: style="color:red"} 
```

![本站favicon](/favicon.ico){:.center}

Hello, *world*{:#world} 

Hello, *world*{: style="color:red"} 

结合自定义的样式，有些场景比较有用。

### Emoji

:camel:
:blush:
:smile:

### Footnotes

This is a text with footnote[^1].

### mermaid

```mermaid
sequenceDiagram
    Alice-->>John: Hello John, how are you?  
    John-->>Alice: Great!
```

```mermaid
graph TD;
  A-->B;
  A-->C;
  B-->D;
  C-->D;
```

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

### mindmap

```mindmap
# topic
## topic2
### topic2.1
### topic2.2
## topic3
<!--Note-->
这是一个备注
<!--/Note-->
### topic3.1
### topic3.2
#### topic3.2.1
#### topic3.2.2
#### topic3.2.3
#### topic3.2.4
#### topic3.2.5
### topic3.4
### topic3.5
### topic3.6
```

[^1]: Here is the footnote 1 definition.
