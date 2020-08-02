# JSON

json是纯文本的，具有层级结构，能够被JavaScript解析 --> JavaScript的eval()方法，可以通过ajax传输，具有“自我描述性”即人类能够读懂，没有结束标签

## 语法

1. 简单值：字符串、数组、布尔值和null，但无undefined
2. 对象
3. 数组

```json
//简单值
"Hello world!"
//对象
{ 
 "name": "Nicholas", 
 "age": 29, 
 "school": { 
 "name": "Merrimack College", 
 "location": "North Andover, MA" 
 } 
}
//数组
[ 
 { 
 "title": "Professional JavaScript", 
 "authors": [ 
 "Nicholas C. Zakas" 
 ], 
 edition: 3, 
 year: 2011 
 }, 
 { 
 "title": "Professional JavaScript", 
 "authors": [ 
 "Nicholas C. Zakas" 
 ], 
 edition: 2, 
 year: 2009 
 }, 
 { 
 "title": "Professional Ajax", 
 "authors": [ 
 "Nicholas C. Zakas", 
 "Jeremy McPeak", 
 "Joe Fawcett" 
 ], 
 edition: 2, 
 year: 2008 
 }, 
 { 
 "title": "Professional Ajax", 
 "authors": [ 
 "Nicholas C. Zakas", 
 "Jeremy McPeak", 
 "Joe Fawcett" 
 ], 
 edition: 1, 
 year: 2007 
 }, 
 { 
 "title": "Professional JavaScript", 
 "authors": [ 
 "Nicholas C. Zakas" 
 ], 
 edition: 1, 
 year: 2006 
 } 
]
```

解析后只用``books[2].title``即可获得上面的第三本书的书名

> JSON 不能存储 Date 对象。
>
> JSON.stringify() 会将所有日期转换为字符串。

## 方法

### JSON.parse()

json数据使用JSON.parse()方法转化为JavaScript对象：

```js
JSON.parse(text,[reviver])
```

- **text**：一个有效的JSON字符串
- **reviver**：可选，一个转换结果的函数

```javascript
var user = JSON.parse(‘{“name”:”张三”,”age”:18}’);
```



### JSON.stringify()

使用JSON.stringify()方法可以把JavaScript对象转为字符串

```js
var JSobj = [“name”,”age”,”createDate”];

var jsonString = JSON.stringify(JSobj); 
```

JSON.stringify()方法更主要的方法是用于过滤

```
JSON.stringify(value,[replacer],space)
```

- **value**:

  必需， 要转换的 JavaScript 值（通常为对象或数组）。

- **replacer**:

  可选。用于转换结果的函数或数组。

  如果 replacer 为函数，则 JSON.stringify 将调用该函数，并传入每个成员的键和值。使用返回值而不是原始值。如果此函数返回 undefined，则排除成员。根对象的键是一个空字符串：""。

  如果 replacer 是一个数组，则仅转换该数组中具有键值的成员。成员的转换顺序与键在数组中的顺序一样。当 value 参数也为数组时，将忽略 replacer 数组。

- **space**:

  可选，文本添加缩进、空格和换行符，如果 space 是一个数字，则返回值文本在每个级别缩进指定数目的空格，如果 space 大于 10，则文本缩进 10 个空格。space 也可以使用非数字，如：\t。



### toJSON(）

原生 Date 对象有一个 toJSON()方法，能够将JavaScript的Date 对象自动转换成ISO 8601日期字符串

即

```js
var book = { 
 "title": "Professional JavaScript", 
 "authors": [ 
 "Nicholas C. Zakas" 
 ], 
 edition: 3, 
 year: 2011, 
 toJSON: function(){ 
 	return this.title; 
 } 
}; 
var jsonText = JSON.stringify(book);
```



## 小例

```javascript
//如何添加多组数据于localStorage里面

//读取数据
function getDate() {
    var data = localStorage.getItem("todo");

    if (data !== null) {
        return JSON.parse(data);
    } else {
        return [];
    }
}

//存储数据
function saveData(local) {
    localStorage.setItem("todo",JSON.stringify(local));
}
```

