# jQuery整理

- 封装了BOM和DOM的JS函数库
- 到官网下载jQuery库，1.x就好

## 一、jQuery的基本使用

- 入口函数
  - 等DOM结构渲染完毕，即可执行内部代码，不必等到所有外部资源加载完成
  - 相当于原生js中的DOMContentLoaded
  - 不同于原生js的load事件是等页面文档，外部js文件、css文件、图片加载完成后才执行的内部代码

```js
//DOM加载完再执行下面代码
//写法一
$(function() {
    ......
});
//写法二
$(document).ready(function(){
    ......
});
```

- 顶级对象$
  - $是jQuery的一个别称，在代码中可以使用jQuery代替$。
  - $是jQuery的顶级对象，类似于原生js的window。(它可以包住所有对象，且有属性和方法)
  - 把元素利用$包装成jQuery对象，就可以用jQuery方法。

- DOM对象和jQuery对象
  - DOM对象：用原生js的document的方法获取过来的对象
  - jQuery对象：用jQuery获取过来的对象。比如`$('div');`
    - jQuery对象本质:利用$对DOM对象包装后产生的对象(伪数组形式存储)
  - DOM对象有DOM对象的方法，jQuery有jQuery的方法，各自不一样。

```js
/*DOM对象和jQuery对象的相互转换，假设有个video标签*/

//1.DOM对象——>jQuery对象
    //(1)直接获取
    $('video');
    //(2)已使用js获取的DOM对象
    var myvideo = document.getElementByTagName('video')[0];
    $(myvideo);
//2.jQuery对象——>DOM对象
//两种方式：$('div')[index]或者$('div').get(index)。index是索引号
    $('video')[0];
    $('video').get(0);
```

## 二、jQuery常用API

### (一)jQuery选择器

- jQuery获取标签的选择器
  - `$("基础选择器")`
    - ID选择器：`$("#id")`
    - 通配符选择器：`$("*")`
    - 类选择器：`$(".class")`
    - 标签选择器：`$("div")`
    - 并集选择器：`$("div,p,li")`
    - 交集选择器：`$("li.select")`
    - 后代选择器：`$("div li")`
    - 子元素选择器：`$("ul li")`
  - `$("筛选选择器")`
    - :first ：`$("div:first")`获取第一个元素
    - :last ：`$("div:last")`获取最后一个元素
    - :eq(index) ：`$("div:eq(2)")`获取到的li元素中，选择索引号为2的元素，从0开始
    - :odd ：`$("div:odd")`获取索引号为**奇数**的元素
    - :even ：`$("div:even")`获取索引号为**偶数**的元素
    - :checked：`$(".mycheckbox:checked")`获取被选中的元素
- jQuery的筛选方法
  - parent()：查找父级。比如`$("li").parent()`
  - parents("选择器")：查找祖先元素。比如`$("p").parents("div")`
  - children("选择器")：选择子元素。比如`$("ul").children("li")`
  - find("选择器)：选择后代元素，比如`$("ul").find("li")`
  - siblings("选择器")：选择兄弟节点，不包括自身(当有多个元素，这一交集，就都包含了)。比如`$("li.second").siblings("li")`
  - nextAll()：选择当前元素之后所有的同辈元素，比如`$("li.first").nextAll()`
  - prevAll()：选择当前元素之前所有的同辈元素`$("li.last").prevAll()`
  - hasClass(class)：检查当前获取的元素是否含有某个特定的类，如果有返回true。比如`$("div").hasClass(".word")`
  - eq(index)：选择当前元素的第几个。比如`$("li").eq(3)`

```html
<body>
  <div>Lorem, ipsum.</div>
  <div>Fuga, ratione?</div>
  <div>Expedita, quidem.</div>
  <div>Pariatur, eveniet.</div>
  <script src="jquery/dist/jquery.min.js"></script>
  <script type="text/javascript">
    //理解一波隐式迭代
    //获取四个div，并设置背景颜色为蓝色，我们不用for循环
    $("div").css("background","blue");
  </script>
</body>
```

### (二)jQuery设置css样式

- jQuery查看样式
  - `$("选择器").css("属性")`
- jQuery修改样式
  - 改单个样式：`$("选择器").css("属性","值")`
  - 改多个样式：`$("选择器").css({"color":"blue","font-size":"20px"})`
    - 加引号吧，可以改的东西比较多。

- ==隐式迭代==
  - 将匹配到的所有元素进行遍历，执行相应的方法，而不用我们去循环。

>元素的显示和隐藏
>1.显示`$("div").show()` 2.隐藏`$("div").hide()`

- jQuery修改元素的类名(==注意别加一点==)
  - 添加类addClass()：`$(".one").addClass("good")`
  - 删除类removeClass()：`$(".one").removeClass("bad")`存在就删
  - 切换类toggleClass()：`$(".one").toggleClass("thing)`有则删，无则加

>jQuery操作类名方式，类似于原生js的classList

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .select{
            background-color: skyblue;

        }
        button{
            background-color: silver;
            padding: 5px;
            font-size: 14px;
            border: 1px solid yellow;
        }
        p{
            display: none;
        }
    </style>
</head>
<body>
    <button>Lorem.</button>
    <button>Aspernatur!</button>
    <button>Assumenda.</button>
    <button>Nam.</button>
    <button>Mollitia.</button>
    <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Facere, at.</p>
    <p>Sapiente cumque minus qui asperiores iusto esse vero. Fugiat, necessitatibus.</p>
    <p>Reiciendis nostrum, saepe aliquid eveniet modi eligendi excepturi pariatur similique!</p>
    <p>Ab quae excepturi officia laborum minima deleniti nulla quidem id.</p>
    <p>Numquam quibusdam nostrum cum quos doloremque reiciendis officiis architecto cumque.</p>

    <script src="jquery/dist/jquery.min.js"></script>
    <script type="text/javascript">
       $(function() {
         //给button绑定事件，体会隐式迭代的方便
           $("button").click(function(){
             //this的addClass()和siblings的removeClass都生效
             //体会一下链式编程的方便
                $(this).addClass("select").siblings("button").removeClass();
             //p标签的显示和隐藏
                $("p").eq($(this).index()).show().siblings("p").hide();
           })
       })
    </script>
</body>
</html>
```

### (三)jQuery封装的动画效果

>[]表示可省略的参数

- 显示隐藏
  - `show([speed],[easing],[fn])`
    - speed：三个默认速度(fast,normal,slow)或者填毫秒数
    - easing：默认是swing，可用参数linear
    - fn：回调函数，动画完成时，每个元素执行一次
  - `hide([speed],[easing],[fn])`
  - `toggle([speed],[easing],[fn])`
- 滑动
  - `slideDown([speed],[easing],[fn])`
  - `slideUp([speed],[easing],[fn])`
  - `slideToggle([speed],[easing],[fn])`
- 淡入淡出
  - `fadeIn([speed],[easing],[fn])`
  - `fadeOut([speed],[easing],[fn])`
  - `fadeToggle([speed],[easing],[fn])`
  - `fadeTo(speed,opacity,[easing],[fn])`：修改透明度，speed和opacity必须写
- 自定义动画
  - `animation(“params”,[speed],[easing],[fn])`
    - params：想要修改的样式属性，以对象形式传递，必须写。(属性名可以不用引号，如果是复合属性则要求用小驼峰命名法)
- **动画队列与停止动画队列的方法**
  - 动画队列:动画队列一旦触发，就会执行。如果多次触发，就造成多个动画排队执行
  - 停止排队：`stop()`
    - 一定要写在动画的前面，相当于结束在此之前的动画，使得动画只执行最后一次。
    - `$("div").stop().slideToggle()`

### (四)jQuery属性操作

- 元素固有属性的操作
  - jQuery查看属性
    - `$("选择器").prop("属性")`
  - jQuery设置属性
    - `$("选择器").prop("属性","属性值")`

>data-attribute为h5提供的可自定义属性。
>其他自己写的也是自定义属性

- 程序员自定义属性
  - jQuery查看属性
    - `$("选择器").attr("自定义属性/data-")`
  - jQuery设置属性
    - `$("选择器").attr("自定义属性/data-")`

- 数据缓存data()
  - data()方法可以在指定的元素上存放数据，并不会修改DOM结构，一旦页面刷新，存放的数据都将被清除
  - 查看数据的方法
    - 比如`$("div").data("name")`
    - 也可以查h5的自定义属性，而且不需要加data-前缀
  - 设置数据的方法
    - 比如`$("div").data("name","lily")`

### (五)jQuery内容文本值

- 普通元素内容html()(相当于原生innerHTML)
  - 查元素内容：`$("div").html()`
  - 改元素内容：`$("div").html("She is so beautiful in white")`
- 普通元素文本内容text()(相当于原生innerText)
  - 查文本内容：`$("div").text()`
  - 改文本内容：`$("div").text("132")`
- 表单元素的值val()(相当于原生的value)
  - 查表单值：`$("input").val()`
  - 改表单值：`$("input").val("12345")`

### (六)jQuery元素操作

- **遍历元素(标签)**
  - 目的：给同一类元素做不同的操作
  - 语法1：`$("div").each(function(index,domElement){……})`
    - index是索引号，domElement是DOM元素对象，不是jQuery
  - 语法2：`$.each(object,function(index,(dom)element){……})`
    - object可以是任何对象(比如数组，对象)，index是索引号/属性名，element是数组内容/属性值

- 创建标签
  - 语法：`$("html标签")`
    - 示例：`$("<div></div>")`

- 添加标签
  - 内部添加
    - `element.append("内容")`：把内容放到element内部的最后面，相当于原生的appendChild()
    - `element.prepend("内容")`：把内容放到element内部的最前面
  - 外部添加
    - `element.before("内容")`：把内容放到element的前面
    - `element.after("内容")`：把内容放到element的后面
- 删除标签
  - `element.remove()`：删除element本身
  - `element.empty()`：删除element的所有子节点
  - `element.html("")`：删除element的元素内容

### (七)jQuery的事件

- 事件对象
  - 绑定事件时候，在回调函数的参数写个变量(比如叫eve)，就可以拿到这个事件对象了`element.on(events,[selector],function(eve){})`
  - 阻止默认事件和阻止冒泡，eve的属性/方法就是原生Js的，像js那样写就好了

- 事件绑定
  - 绑定单一事件：
    - 语法：`element.event(function(){})`
    - 比如：`$("div").click(function(){})`
    - 其他事件和原生js基本一样:mouseover、mouseout、blur、foucs、change、keydown、resize、keyup、scroll等
  - 同时绑定单个或多个事件`element.on()`：
    - 写法一：事件处理不同

        ```js
          $("div").on({
            click:function() {
              $(this).css("background","skyblue");
            },
            mouseenter:function(){
              $(this).css("color","white");
            },
            mouseleave:function(){
              $(this).css("color",)
            }
          })
        ```

    - 写法二：事件处理一样

      ```js
        //1.写法$("选择器").on(events,function(){})
        $("div").on("mouseenter mouseleave",function(){
          $(this).toggleClass("current");
        })
        //2.多加一个参数，实现事件委派.
        //$("选择器").on(events,"选择器(选子元素)",function(){})
        //比如$("ul li").click(function(){})可以委派给ul。
        //委派好处是：新添加的li不用重新绑定
        //以前有bind()，live()，delegate()等方法来处理委派，现在都用on的，所有用它就行了
        $("ul").on("click","li",function(){

        })
        //上面因为锁定了li，如果ul有其他不是li的子元素，点了没用
        //新增的li，点了也依然有用
      ```

- 事件解绑
  - 解绑所有事件:`$("选择器").off()`。比如`$("div").off()`
  - 解绑某个事件：`$("选择器").off("事件")`
  - 解绑事件委托：`$("选择器").off("事件","元素")`

- jQuery封装的只执行一次事件的方法
  - 语法与on()基本一致
  - 即使隐式迭代，也只执行一次。

- 自动触发事件三种方式
  - 第一种：`$("选择器").事件()`直接调用函数
  - 第二种：`$("选择器").trigger("事件")`
  - 第三种：`$("选择器").triggerHandler("事件")`
    - triggerHandler比较特殊，不会触发元素的默认行为

### (八)jQuery尺寸、位置操作

- jQuery尺寸
  - `width()/height()`：width/height的值
  - `innerWidth()/innerHeight()`：width+padding/height+padding的值
  - `outerWidth()/outerHeight()`：width+padding+border/height+padding+border
  - `outerWidth(true)/outerHeight(true)`：width+padding+border+margin/height+padding+border+margin。这种改不了
  - 1.参数为空，是获取相应值，返回数字型
  - 2.如果参数为数字，则只是修改width/height，直到width/height为0，再改也不变了。且返回被修改的标签
- jQuery的位置
  - `offset()`：距离可视窗口边缘距离。
    - 不修改参数，返回对象，含一个left，一个top属性
    - 修改参数：`offset({left:500,top:400})`
  - `position()`：返回定位元素，距离祖先定位元素的距离。(若非定位，返回距离可视窗口的距离)
    - **这个方法只用来获取，不能设置**
  - `scrollTop()/scrollLeft()`：被卷去的头部/被卷曲的左侧

### (九)jQuery其他方法

- jQuery对象拷贝
  - 语法：`$.extent([deep],target,object1,[objectN])`
    - deep: 设置为true就深拷贝，默认浅拷贝
    - target：拷贝后放在这个变量
    - object1：它的内容将被拷贝
    - [objectN]：它的内容也是要拷贝的，拷完合并在target里面

- jQuery多库共存
  - 问题概述:jQuery用$作为标识符，随着jQuery的流行，其他js库也有用$作为标识符，这样就会有冲突
  - 客观需求：需要一个解决方案，让jQuery和其他库不存在冲突，可以同时存在，这就是多库共存
  - jQuery解决方案
    - 方法一：把里面的$符号换成jQuery就好了
    - 方法二：自定义函数名.`var my = $.noConflict()`使用`my()`

## 三、jQuery插件

- jQuery插件网站
  - jQuery插件库：http://www.jq22.com/
  - jQuery之家：http://www.htmleaf.com/