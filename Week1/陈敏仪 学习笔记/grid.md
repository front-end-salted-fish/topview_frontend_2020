## grid

#### 块级容器

1、display：grid；

2、使用 `grid-template-columns` 规则可划分列数，使用 `grid-template-rows` 划分行数。

- 固定宽度（具体像素）当容器宽度过大时将漏白。

- 百分比自动适就容器。

- 使用 `repeat` 统一设置值，第一个参数为重复数量，第二个参数是重复值。

- 自动填充,根据容器尺寸，自动设置元素尺寸。

  `grid-template-columns：repeat(auto-fill,100px);`

- 使用 `fr` 单位设置元素在空间中所占的比例。

  `grid-template-columns：1fr 2fr;``//1:2`

  `grid-template-columns：repeat(2,1fr 2fr);//1:2:1:2`

- 组合定义

  `grid-template：1fr 2fr;`

- minmax方法可以设置取值范围

#### 间距

1、使用 `row-gap` 设置行间距。

2、使用 `column-gap` 定义列间距。

3、`gap： 20px 10px;` // 行  列

行列间距相等则致谢一个值；

#### 元素定位

1、根据容器已经画好的行列来摆放

- `grid-row-start`：上边框所在的水平网格线
- `grid-row-end`：下边框所在的水平网格线
- `grid-column-start`：左边框所在的垂直网格线
- `grid-column-end`：右边框所在的垂直网格线

2、简写

`grid-row:2/4;`//从第二行开始到第四行结束

```
grid-column:2/4;
```

3、根据偏移量

使用 `span` 可以设置移动单元格数量，数值只能为正数。

//假设容器三行三列，则示例中元素处于中间

```
 grid-row-start: 2;
 grid-column-start: 2;
 grid-row-end: span 1;
 grid-column-end: span 1;
```

4、grid-area

```
grid-row-start/grid-column-start/grid-row-end/grid-column-end
```

#### 对齐

**设置给容器**

1、控制整行/列元素在栅格里的对齐

- justify-items  水平方向

- align-items  垂直方向

- place-items：align-items  justify-items；

  可选值：

  start：对其单元格的起始边缘

  end：对齐单元格的结束边缘

  center：单元格内居中

  stretch：占满整个单元格

  示例：

  `justify-items：start；`//每一横行的元素都紧贴着单元格的左侧

2、整个内容区在容器里的位置

- justify-content  水平方向

- align-content  垂直方向

- place-content：align-content  justify-content；

  可选值：

  合在一起移动：

  start

  end

  center

  stretch

  两侧有间距：

  space-around

  space-between：紧贴容器壁

  space-evenly：等分

**设置给子元素**

3、单个元素在单元格内的位置

- justify-self  水平方向

- align-self   垂直方向

- place-self ：align-self   justify-self ；

  可选值：

  start：对其单元格的起始边缘

  end：对齐单元格的结束边缘

  center：单元格内居中

  stretch：占满整个单元格

  