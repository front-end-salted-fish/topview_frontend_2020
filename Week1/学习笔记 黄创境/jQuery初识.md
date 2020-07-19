# jQuery初识

因为时间紧，所以也都是摘抄一些网上的，有时间填一填这个坑

## 一、简介

jQuery 是一个快速、轻量级、可扩展的 js 库，它提供了易于使用的跨浏览器的API，使得

访问dom，时间处理、动画效果、ajax请求变得简单。

简化了JS对DOM的操作



## 二、jQuery获取方式

[jQuery官网下载链接](https://jquery.com/download/)

又或者直接网上安装

```HTML
<head>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js">
</script>
</head>
```



## 三、jQuery核心

### 1、jQuery核心对象

> window.jQuery = window.$ = jQuery;

- 在window对象中，多了两个属性，叫做jQuery 和 `$`
- `jQuery`属性 和 `$`可以相互替代

### 2、关于jQuery对象

使用jQuery获取的元素是jQuery对象，**不是DOM对象，不能使用DOM对象方法**。但**DOM对象可以和JQuery对象相互转换**

dom转jQuery：``$(DOM对象)``

jQuery转dom：``$('div')[index]`` 或者``$('div').get(index)``



### 3、**注意点**

#### 3-1 关于赋值操作

**任何jQuery 元素对象的赋值操作，基本上都是通过方法的第二个参数赋值，不会出现`=`**

#### 3-2 `$ is not defined`错误

- jquery 的js文件没有导入
- jquery 的js文件路径错误
- jquery 的js文件导入位置错误



## 五、访问节点

### 1、基本选择器

基本语法：`$(选择器)`

**选择器语法 和 CSS 选择器语法一致**



### 2、后代选择器

- `$("div>a")` `$("div a")`
- `$().find(选择器)` 在某个元素的后代中查找
- `$().children(选择器)` 选择器可以不写，默认找所有子元素，否则找满足选择器的子元素



```javascript
$(function(){
            // 后代中的h1
            let h1s = $("#d").find("h1");
            console.log(h1s);
            // 所有的子元素
            h1s= $("#d").children();
            console.log(h1s);
            // 子元素中的h1
            h1s= $("#d").children("h1");
            console.log(h1s);
        })
```



### 3、父选择器

`$().parent()` 找某个节点的 父节点

### 4、兄弟节点

|        **方法**         |               **作用**               |
| :---------------------: | :----------------------------------: |
|     $().siblings()      |             所有兄弟节点             |
|       $().next()        |            下一个兄弟节点            |
|       $().prev()        |            上一个兄弟节点            |
|      $().nextAll()      |          下面的所有兄弟节点          |
|      $().prevAll()      |          上面的所有兄弟节点          |
| $().nextUntil(selector) | 介于两个节点之间的所有节点（向下找） |
| $().prevUntil(selector) | 介于两个节点之间的所有节点（向上找） |

### 5、`:`选择器

- **$(“span:first”)**	选择第一个span
- **$(“span:last”)**	选择最后一个span
- **$(“span:even”)**	第偶数个span
- **$(“span:odd”)**	第奇数个span
- **$(“input:button”)或者\$(“:button”)**	所有的input 并且type是button的元素
- **$(“:checkbox”)**	所有的 复选框
- **$(“:radio”)**	所有的 单选按钮
- **$(“:checked”)**	所有选中的checkbox / radio
- **$(“:selected”)**	所有选中的select>option


### 6、选择器过滤

- **$().first()**	满足选择器的第一个元素
- **$().last()**	满足选择器的最后一个元素
- **$().eq(n)**	满足选择器的第n个元素（从0开始）
- **$(selector1).not(selector2)**	满足selector1并且不满足selector2的元素



## 六、访问属性

- DOM属性
  - `$().prop("属性名") 取值操作`
  - `$().prop("属性名","属性值") 赋值操作`
- HTML属性
  - `$().attr("属性名")` 取值操作
  - `$().attr("属性名","属性值")` 赋值操作



## 七、访问样式

### 1、访问class

- 作为普通属性访问（第六章）
- `addClass("样式名")` 添加样式
- `removeClass("样式名")` 删除样式
- `removeClass()` 删除所有样式
- `toggleClass`(“样式名”) 有则删除，没有则添加

### 2、访问css

- `$().css("样式名")` 取值操作 ( 原始css样式 background-color )
- `$().css("样式名","样式值")` 赋值操作
- `$().css({})` 一次性修改多个css样式 参数是json对象，对象属性名是css属性名，对象值是css属性值
- `$().width()` 、 `$().height()` 无参取值，有参赋值

## 八、访问内容

- `$().html()` 无参取值，有参赋值，注意：取值取标签，赋值解析标签
- `$().text()` 无参取值，有参赋值，注意：取值不取标签，赋值不解析标签
- `$().val()` 无参取值，有参赋值，相当于表单组件的value



## 九、jquery事件绑定

### jquery动态事件绑定

- `$().事件名(事件处理函数)` `$("#d").click(function(){})`
- `$().on("事件名",事件处理函数)` `$("#d").on("click",function(){})`
- ``$().bind("事件名",事件处理函数)``    ``$("#d").bind("click",function(){})``

### 解除事件绑定

``$().unbind("事件名")``

### 使用js触发事件

``$().事件名()``



## 十、DOM操作

### 添加

```javascript
$(a).append(b)

$(a).prepend(b)

$(a).after(b)

$(a).before(b)
```

### 删除

- `$(a).remove()` 删除a以及子标签
- `$(a).empty()` 删除a的子标签，a不删除



## 十一、jQuery动画效果

需要记住的`hide()` 和 `show()`