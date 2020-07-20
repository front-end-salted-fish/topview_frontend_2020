## Basic Javascript

**2020/07/13**

1、有两种方式访问对象属性，一个是点操作符(`.`)，一个是中括号操作符(`[]`)。

​	当你知道属性的名称的时候，使用点操作符。

​	想访问的属性的名称有一个空格，只能使用中括号操作符(`[]`)。

​	注意中括号内要加双引号；

**2020/7/14**

1、`Math.random()`用来生成一个在0(包括0)到1(不包括1)之间的随机小数

2、Math.floor(10*Math.random());//生成0到9之间的整数

3、生成随机数是在两个指定的数之间。

```js
function randomRange(myMin, myMax) { 

   return Math.floor(Math.random() * (myMax -myMin + 1)) + myMin; 

}
```

4、求数组的值（n指定个数），递归

```js
function sum(arr, n) {
	if (n <= 0) {
		return 0;
	}
	if(n>0){
		return sum(arr, n-1)+arr[n-1];
	}
}
```

5、根据n倒序插入数组，递归

```javascript
function countdown(n){
  if (n == -1) {
    return [];
  }
  if(n>=0){
    const countArray = countdown(n - 1);
    if(n!=0) {
      countArray.unshift(n);
    }
    return countArray;
  }
}
```

6、根据范围返回数组，递归

```js
function rangeOfNumbers(startNum, endNum) {
  if(startNum==endNum) {
    return [startNum];
  }else{
    var arr=[];
    arr=rangeOfNumbers(startNum, endNum-1);
    arr.push(endNum);
    return arr;
  }
};
//rangeOfNumbers(6, 9) should return [6, 7, 8, 9].
```



## 数组

**类型转换**

1、将数组转变为字符串

- toString( )
- String( )
- join( )

2、Array.from( )

​	将类数组转换为数组，

​	类数组指包含length 属性或可迭代的对象。

3、拓展运算符**...**

**管理元素**

1、push( )

2、pop( )

3、shift( )

4、unshift( )

5、fill( )

6、slice( )  

​	返回一个新数组

​	参一：截取的起始位置

​	参二：截取的结束位置

​	不设置参数：获取所有元素

7、splice( )

​	返回一个数组，数组包含从原数组删去的项，没删除则返回空数组。

​	功能：删除、插入、替换

8、[ ]  清空数组

**合并拆分**

1、join

2、split

​	将字符串分割成数组

3、concat

​	先复制当前数组再拼接

4、copyWithin

​	从数组中复制一部分到同数组中的另外位置。

**查找元素**

1、indexOf( )

2、lastIndexOf( )

3、includes( )

​	返回值是布尔类型

​	查找字符串用这个更方便

4、find( )

​	可查找引用类型

​	find 方法找到后会把值返回出来，如果找不到返回值为undefined

​	返回第一次找到的值，不继续查找

5、findIndex( )

​	返回索引值，找不到则返回-1

**数组排序**

1、reverse( ) 逆序

2、sort( )    默认升序

**迭代方法**

1、every( )

2、filter( )

3、forEach( )

4、map( )

5、some( )

**归并方法**

1、reduce( )

2、reduceRight( )



**总结JavaScript中哪些元素可以被遍历？**

字符串，数组，伪数组，对象

**有哪些方法可以遍历？不同遍历方法的使用场景？**

1、for循环

2、while循环

3、do while循环

4、forEach循环  

​	只能用来遍历数组

​	遍历操作数组的每一个元素，无返回值

```js
let divList = document.querySelectorAll('div');   //divList不是数组，而是nodeList
 
//进行转换后再遍历
[].slice.call(divList).forEach(function(element,index){
  element.classList.add('test')
})
 
Array.prototype.slice.call(divList).forEach(function(element,index){
  element.classList.remove('test')
})
 
[...divList].forEach(function(element,index){   //ES6写法
  //do something
})
```

5、map( )方法  

​	 只能用来遍历数组

​	返回一个新数组，数组中的元素为原始数组元素调用函数处理后的值。

```js
let arr = [1,2,3];
let tt = arr.map(function(i){
 console.log(i)
 return i*2;
})
```

6、filter( )方法

​	返回通过过滤的元素

```js
let arr = [1,2,3];
let tt = arr.filter(function(i){
 return i>1;
})
// [2,3]
```

7、some( )方法

​	返回 boolean 值

​	查找数组中是否存在符合条件的元素，有一个元素符合了就立刻返回。

8、every( )方法

​	返回 boolean 值

​	查找数组中的每一个元素，看是否都符合条件。

9、reduce( )方法

​	迭代器，从头到尾

10、reduceRight( )方法

​	从数组末尾开始，其他与reduce( )基本一致。

11、for of 循环

​	可遍历 Arrays（主要）, Strings， Maps、Sets等可迭代(Iterable data)的数据结构

```js
let arr = ['name','age'];
for(let i of arr){
 console.log(i)
}
// name
// age
```

12、for in 循环

​	主要用于遍历普通对象

​	在使用for in 遍历domList时，需要将domList转换为数组

​	类似这样的对象还有函数的属性arguments对象，字符串也可以遍历

```js
var res = [].slice.call(domList);
for(var item in res) {}
let obj = {name:'zhou',age:'12'}
for(let i in obj){
 console.log(i,obj[i])
}
// name zhou
// age 12
//i在遍历数组时，指的是元素的索引值/下标；
//i在遍历对象时，指的是键值对的键（key）；
```

