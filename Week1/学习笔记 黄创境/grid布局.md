# Grid布局 #
即网格布局

## 容器 ##
和flex类似，需要给个容器
```
div {
  display: grid;
}
```
对于网格布局
如图
![详情见http://www.ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html](https://www.wangbase.com/blogimg/asset/201903/1_bg2019032503.png)


## grid-template ##

### 行与列 ###
设置行宽与宽高，单位可以用px、%、fr、em、还有``repeat()``进行方便改写

> 列 : ``grid-template-columns``
> 行 : ``grid-template-rows``

```
.container {
  display: grid;
  grid-template-columns: 100px 100px 100px;
  grid-template-rows: 100px 100px 100px;
}
```

```
.container {
  display: grid;
  grid-template-columns: 1fr 1fr;
}
```

```
.container {
  display: grid;
  grid-template-columns: repeat(3, 33.33%);
  grid-template-rows: repeat(3, 33.33%);

  //或者重复某一模式 grid-template-columns: repeat(2, 100px 20px 80px);
}


```

**自动填充列数可以用``auto-fill``关键字**
```
.container {
  display: grid;
  grid-template-columns: repeat(auto-fill, 100px);
}
```

**定义长度范围``minmax()``**
```
grid-template-columns: 1fr 1fr minmax(100px, 1fr);
```

**自动填充长度``auto``**
```
grid-template-columns: 100px auto 100px;
```

### 网格线命名 ###

``grid-template-columns``属性和``grid-template-rows``属性里面，还可以使用方括号，指定每一根网格线的名字，方便以后的引用。

```
.container {
  display: grid;
  grid-template-columns: [c1] 100px [c2] 100px [c3] auto [c4];
  grid-template-rows: [r1] 100px [r2] 100px [r3] auto [r4];
}
```

### grid-template ###
```
/* 为 grid-template-rows / grid-template-columns */
  grid-template: 1fr 1fr/ifr 1fr; 
```

### grid-gap ###
设置间隔

```
.container {
  grid-row-gap: 20px;
  grid-column-gap: 20px;
}
```
又或者```grid-gap: <grid-row-gap> <grid-column-gap>;```

```
.container {
  grid-gap: 20px 20px;
}
```

## grid-template-areas ##
设置区域

```
.container {
  display: grid;
  grid-template-columns: 100px 100px 100px;
  grid-template-rows: 100px 100px 100px;
  grid-template-areas: 'a b c'
                       'd e f'
                       'g h i';
}
```

> 有区域不用时，用``.``表示

## grid-auto-flow ##
设置顺序

```
grid-auto-flow: row;		//默认。先行后列
grid-auto-flow: column;			//先列后行
grid-auto-flow: row dense;		//先行后列，并且尽可能紧密填满，尽量不出现空格。
grid-auto-flow: column dense;		//先列后行，并且尽可能紧密填满，尽量不出现空格。

```

## items ##
分``justify-items``、``align-items``和``place-items``

设置单元格内容的位置
```
.container {
  justify-items: start | end | center | stretch;
  align-items: start | end | center | stretch;
}

```

其中
- start：对齐单元格的起始边缘。
- end：对齐单元格的结束边缘。
- center：单元格内部居中。
- stretch：拉伸，占满单元格的整个宽度（默认值）。

``place-items``即简写

```
place-items: <align-items> <justify-items>;
```

## content ##

分``justify-content``、``align-content``和``place-content``

设置内容的位置

```
.container {
  justify-content: start | end | center | stretch | space-around | space-between | space-evenly;
  align-content: start | end | center | stretch | space-around | space-between | space-evenly;  
}
```

- start - 对齐容器的起始边框。
- end - 对齐容器的结束边框。
- center - 容器内部居中。
- stretch - 项目大小没有指定时，拉伸占据整个网格容器。
- space-around - 每个项目两侧的间隔相等。所以，项目之间的间隔比项目与容器边框的间隔大一倍。
- space-between - 项目与项目的间隔相等，项目与容器边框之间没有间隔。
- space-evenly - 项目与项目的间隔相等，项目与容器边框之间也是同样长度的间隔。

``place-content``即简写

```
place-content: <align-content> <justify-content>；
```

## 项目属性 ##
### 定位 ###
``grid-column-start`` 属性 
> 左边框所在的垂直网格线


``grid-column-end`` 属性
> 右边框所在的垂直网格线


``grid-row-start`` 属性
> 上边框所在的水平网格线


``grid-row-end`` 属性
> 下边框所在的水平网格线

```
.item-1 {
  grid-column-start: 1;
  grid-column-end: 3;
  grid-row-start: 2;
  grid-row-end: 4;
}
```

**span代表跨域**

```
.item-1 {
  grid-column-start: span 2;
}
```
> 会产生重叠

**简写**
```
.item-1 {
  grid-column: 1 / 3;
  grid-row: 1 / 2;
}
/* 等同于 */
.item-1 {
  grid-column-start: 1;
  grid-column-end: 3;
  grid-row-start: 1;
  grid-row-end: 2;
}
```

```
.item-1 {
  grid-column: 1 / 3;
  grid-row: 1 / 3;
}
/* 等同于 */
.item-1 {
  grid-column: 1 / span 2;
  grid-row: 1 / span 2;
}
```

** grid-area**
```

.item {
  grid-area: <row-start> / <column-start> / <row-end> / <column-end>;
}
```

### 位置 ###
``justify-self``属性设置单元格内容的水平位置（左中右），跟``justify-items``属性的用法完全一致，但只作用于单个项目。

``align-self``属性设置单元格内容的垂直位置（上中下），跟``align-items``属性的用法完全一致，也是只作用于单个项目。

```
.item {
  justify-self: start | end | center | stretch;
  align-self: start | end | center | stretch;
}
```

**place-self简写**
```
place-self: <align-self> <justify-self>;
```


