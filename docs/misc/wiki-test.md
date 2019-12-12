# MkDocs-Material 效果测试

## Markdown 功能测试

### 列表测试
* 水果颜色和价格
  * 苹果
    * 红色
    * 8元
  * 香蕉
    * 黄色
    * 5元
* 水果购买优先级
1. 苹果
2. 香蕉

### 任务列表测试
- [ ] 未完成
- [x] 已完成

### 引用测试
> 这是一条引用
> 
### 代码测试

`single line`

```ruby
require 'redcarpet'
markdown = Redcarpet.new("Hello World!")
puts markdown.to_html
```
### 数学公式
$$
\mathbf{V}_1 \times \mathbf{V}_2 =  \begin{vmatrix}
\mathbf{i} & \mathbf{j} & \mathbf{k} \\
\frac{\partial X}{\partial u} &  \frac{\partial Y}{\partial u} & 0 \\
\frac{\partial X}{\partial v} &  \frac{\partial Y}{\partial v} & 0 \\
\end{vmatrix}
$$

$x+y=k$

### 表格测试
| First Header  | Second Header |
| ------------- | ------------- |
| Content Cell  | Content Cell  |
| Content Cell  | Content Cell  |

### 脚注测试
这是一个脚注[^footnote].
[^footnote]: 这里是脚注的文本

### 水平分割线
-----

### 链接测试
[Doragd](https://github.com/doragd)

### 图片测试
![img](https://www.baidu.com/img/baidu_jgylogo3.gif)

### 文字效果
**加粗**

*斜体*

~~删除线~~

:smile:


## Material 插件效果
### admonition

!!! note
    这是一个笔记框

!!! note "这是它的标题"
    这是一个笔记框

!!! note ""
    空标题

!!! note ""
    代码嵌入
    ```python
    import os
    print("test")
    ```

??? note "折叠笔记块"
    这是一条可以展开和折叠的笔记

#### 不同类型的框
!!! abstract
    Lorem ipsum dolor sit amet, consectetur adipiscing elit.

!!! info
    Lorem ipsum dolor sit amet, consectetur adipiscing elit.

!!! tip
    Lorem ipsum dolor sit amet, consectetur adipiscing elit.

!!! success
    Lorem ipsum dolor sit amet, consectetur adipiscing elit.

!!! question
    Lorem ipsum dolor sit amet, consectetur adipiscing elit.

!!! warning
    Lorem ipsum dolor sit amet, consectetur adipiscing elit.

!!! failure
    Lorem ipsum dolor sit amet, consectetur adipiscing elit.

!!! danger
    Lorem ipsum dolor sit amet, consectetur adipiscing elit.

!!! bug
    Lorem ipsum dolor sit amet, consectetur adipiscing elit. 
  
!!! example
    Lorem ipsum dolor sit amet, consectetur adipiscing elit.

!!! quote
    Lorem ipsum dolor sit amet, consectetur adipiscing elit. 

### CodeHilite
#### 高亮指定行
``` python hl_lines="3 4"
""" Bubble sort """
def bubble_sort(items):
    for i in range(len(items)):
        for j in range(len(items) - 1 - i):
            if items[j] > items[j + 1]:
                items[j], items[j + 1] = items[j + 1], items[j]
```






        
## Material 颜色主题切换

### Primary colors 主色

> 默认为 `white` 

点击色块可更换主题的主色

<div id="color-button">
<button data-md-color-primary="red">Red</button>
<button data-md-color-primary="pink">Pink</button>
<button data-md-color-primary="purple">Purple</button>
<button data-md-color-primary="deep-purple">Deep Purple</button>
<button data-md-color-primary="indigo">Indigo</button>
<button data-md-color-primary="blue">Blue</button>
<button data-md-color-primary="light-blue">Light Blue</button>
<button data-md-color-primary="cyan">Cyan</button>
<button data-md-color-primary="teal">Teal</button>
<button data-md-color-primary="green">Green</button>
<button data-md-color-primary="light-green">Light Green</button>
<button data-md-color-primary="lime">Lime</button>
<button data-md-color-primary="yellow">Yellow</button>
<button data-md-color-primary="amber">Amber</button>
<button data-md-color-primary="orange">Orange</button>
<button data-md-color-primary="deep-orange">Deep Orange</button>
<button data-md-color-primary="brown">Brown</button>
<button data-md-color-primary="grey">Grey</button>
<button data-md-color-primary="blue-grey">Blue Grey</button>
<button data-md-color-primary="white">White</button>
</div>

<script>
  var buttons = document.querySelectorAll("button[data-md-color-primary]");
  Array.prototype.forEach.call(buttons, function(button) {
    button.addEventListener("click", function() {
      document.body.dataset.mdColorPrimary = this.dataset.mdColorPrimary;
      localStorage.setItem("data-md-color-primary",this.dataset.mdColorPrimary);
    })
  })
</script>

### Accent colors 辅助色

> 默认为 `red` 

点击色块更换主题的辅助色

<div id="color-button">
<button data-md-color-accent="red">Red</button>
<button data-md-color-accent="pink">Pink</button>
<button data-md-color-accent="purple">Purple</button>
<button data-md-color-accent="deep-purple">Deep Purple</button>
<button data-md-color-accent="indigo">Indigo</button>
<button data-md-color-accent="blue">Blue</button>
<button data-md-color-accent="light-blue">Light Blue</button>
<button data-md-color-accent="cyan">Cyan</button>
<button data-md-color-accent="teal">Teal</button>
<button data-md-color-accent="green">Green</button>
<button data-md-color-accent="light-green">Light Green</button>
<button data-md-color-accent="lime">Lime</button>
<button data-md-color-accent="yellow">Yellow</button>
<button data-md-color-accent="amber">Amber</button>
<button data-md-color-accent="orange">Orange</button>
<button data-md-color-accent="deep-orange">Deep Orange</button>
</div>

<script>
  var buttons = document.querySelectorAll("button[data-md-color-accent]");
  Array.prototype.forEach.call(buttons, function(button) {
    button.addEventListener("click", function() {
      document.body.dataset.mdColorAccent = this.dataset.mdColorAccent;
      localStorage.setItem("data-md-color-accent",this.dataset.mdColorAccent);
    })
  })

  // #758
  document.getElementsByClassName('md-nav__title')[1].click()
</script>