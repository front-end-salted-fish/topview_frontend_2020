

# 模拟jq的``$``

[详情见阮一峰的网络日志《如何做到 jQuery-free？》]: http://www.ruanyifeng.com/blog/2013/05/jquery-free.html



```js
/*简便代码*/
var query = document.querySelector.bind(document);
var queryAll = document.querySelectorAll.bind(document);
var fromId = document.getElementById.bind(document);
```



## 为什么要`bind` 对象?

没有bind的使用会报错

```json
var select = document.querySelectorAll;
select('div');
```



```error
Uncaught TypeError: Illegal invocation
```

错误显示非法调用



## 原因

``document`` 是类的一个实例而 ``querySelectorAll`` 是原型链的方法，**通过实例来运行原型链上的方法时，解释器会自动将this指向那个实例**



但

```json
var select = document.querySelectorAll;
```

你的变量仅仅是指向了原型链上的那个函数，而没有绑定this指针，所以你才需要在外面手动绑定一下指针。



```js
select.call(document, 'div'); 		//成功
```



即 **如果不bind，函数体里的this将指向全局对象而并不是document对象。**



bind的作用是，创建一个新函数，称为绑定函数。当调用这个绑定函数时，绑定函数会以创建它时传入 bind()方法的第一个参数作为 this，传入 bind() 方法的第二个以及以后的参数。

而

```js
var $ = document.querySelector.bind(document);
```



相当于

```js
var $ = function(document) {
  return function() {
    return document.querySelectorAll.call(document, arguments);
  }
}
```





