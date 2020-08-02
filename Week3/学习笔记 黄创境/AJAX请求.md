# AJAX

AJAX = 异步 JavaScript 和 XML。

AJAX 是一种用于创建快速动态网页的技术。

通过在后台与服务器进行少量数据交换，AJAX 可以使网页实现异步更新。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。

ajax包括以下几步骤：1、创建AJAX对象；2、发出HTTP请求；3、接收服务器传回的数据；4、更新网页数据

## XMLHttpRequest **对象**

### 创建xml对象

```js
 var xmlhttp;
  if (window.XMLHttpRequest)
  {
    //  IE7+, Firefox, Chrome, Opera, Safari 浏览器执行代码
    xmlhttp=new XMLHttpRequest();
  }
  else
  {
    // IE6, IE5 浏览器执行代码
    xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
  }
```



### 发送请求

**open()**

　　在使用XHR对象时，要调用的第一个方法是open()，如下所示，该方法接受3个参数

```js
xhr.open("get","example.php", false);
```

1、open()方法的**第一个参数**用于指定发送请求的方式，这个字符串，不区分大小写，但通常使用大写字母。"GET"和"POST"是得到广泛支持的

2、open()方法的**第二个参数**是URL，该URL相对于执行代码的当前页面，且只能向同一个域中使用相同端口和协议的URL发送请求。如果URL与启动请求的页面有任何差别，都会引发安全错误

3、open()方法的**第三个参数**是表示是否异步发送请求的布尔值，如果不填写，默认为true，表示异步发送

**send()**

　　send()方法接收一个参数，即要作为请求主体发送的数据。调用send()方法后，请求被分派到服务器

　　如果是GET方法，send()方法无参数，或参数为null；如果是POST方法，send()方法的参数为要发送的数据

```js
xhr.open("get", "example.txt", false);
xhr.send(null);
```



### 接收响应

　　一个完整的HTTP响应由状态码、响应头集合和响应主体组成。在收到响应后，这些都可以通过XHR对象的属性和方法使用，主要有以下4个属性

- **responseText**: 作为响应主体被返回的文本(文本形式)
- **responseXML**: 如果响应的内容类型是'text/xml'或'application/xml'，这个属性中将保存着响应数据的XML DOM文档(document形式)
- **status:** HTTP状态码(数字形式)
- **statusText:** HTTP状态说明(文本形式)

　　在接收到响应后，第一步是检查status属性，以确定响应已经成功返回。一般来说，可以将HTTP状态码为200作为成功的标志。此时，responseText属性的内容已经就绪，而且在内容类型正确的情况下，responseXML也可以访问了。此外，状态码为304表示请求的资源并没有被修改，可以直接使用浏览器中缓存的版本；当然，也意味着响应是有效的

　　无论内容类型是什么，响应主体的内容都会保存到responseText属性中，而对于非XML数据而言，responseXML属性的值将为null

```js
if((xhr.status >=200 && xhr.status < 300) || xhr.status == 304){
    alert(xhr.responseText);
}else{
    alert('request was unsuccessful:' + xhr.status);
}
```



### 同步

　　如果接受的是同步响应，则需要将open()方法的第三个参数设置为false，那么send()方法将阻塞直到请求完成。一旦send()返回，仅需要检查XHR对象的status和responseText属性即可

　　同步请求是吸引人的，但应该避免使用它们。客户端javascript是单线程的，当send()方法阻塞时，它通常会导致整个浏览器UI冻结。如果连接的服务器响应慢，那么用户的浏览器将冻结

```js
    //创建xhr对象
    var xhr;
    if(window.XMLHttpRequest){
        xhr = new XMLHttpRequest();
    }else{
        xhr = new ActiveXObject('Microsoft.XMLHTTP');
    }
    //发送请求
    xhr.open('get','/uploads/rs/26/ddzmgynp/message.xml',false);
    xhr.send();
    //同步接受响应
    if(xhr.readyState == 4){
        if(xhr.status == 200){
            //实际操作
            result.innerHTML += xhr.responseText;
        }
    }
```

可以通过**readyState**来查看请求/响应阶段

- 0：未初始化。尚未调用 open()方法。

- 1：启动。已经调用 open()方法，但尚未调用 send()方法。

- 2：发送。已经调用 send()方法，但尚未接收到响应。

- 3：接收。已经接收到部分响应数据。

- 4：完成。已经接收到全部响应数据，而且已经可以在客户端使用了。



理论上，**只要readyState属性值由一个值变成另一个值，都会触发一次readystatechange事件。**可以利用这个事件来检测每次状态变化后readyState的值。通常，我们对readyState值为4的阶段感兴趣，因为这时所有数据都已就绪



**必须在调用open()之前指定onreadystatechange 事件处理程序才能确保跨浏览器兼容性**，否则将无法接收readyState属性为0和1的情况

```js
xhr.onreadystatechange = function(){
    if(xhr.readyState === 4){
        if(xhr.status == 200){
            alert(xhr.responseText);
        }
    }
}
```



### 异步

　　如果需要接收的是异步响应，这就需要检测XHR对象的readyState属性

```js
    //创建xhr对象
    var xhr;
    if(window.XMLHttpRequest){
        xhr = new XMLHttpRequest();
    }else{
        xhr = new ActiveXObject('Microsoft.XMLHTTP');
    }
    //异步接受响应
    xhr.onreadystatechange = function(){
        if(xhr.readyState == 4){
            if(xhr.status == 200){
                //实际操作
                result.innerHTML += xhr.responseText;
            }
        }
    }
    //发送请求
    xhr.open('get','message.xml',true);
    xhr.send();
```



### 超时

　　XHR对象的timeout属性等于一个整数，表示多少毫秒后，如果请求仍然没有得到结果，就会自动终止。该属性默认等于0，表示没有时间限制

　　如果请求超时，将触发ontimeout事件

```js
xhr.open('post','test.php',true);
xhr.ontimeout = function(){
    console.log('The request timed out.');
}
xhr.timeout = 1000;
xhr.send();
```



### 重写

​	overrideMimeType()方法，用于重写 XHR 响应的 MIME 类型。

​	比如，服务器返回的 MIME 类型是 text/plain，但数据中实际包含的是 XML。根据 MIME 类型，

即使数据是 XML，responseXML 属性中仍然是 null。通过调用 overrideMimeType()方法，可以保

证把响应当作 XML 而非纯文本来处理。

```js
var xhr = createXHR(); 
xhr.open("get", "text.php", true); 
xhr.overrideMimeType("text/xml"); 
xhr.send(null);
```



### 进度事件

- **loadstart**：在接收到响应数据的第一个字节时触发。

- **progress**：在接收响应期间持续不断地触发。

- **error**：在请求发生错误时触发。

- **abort**：在因为调用 abort()方法而终止连接时触发。

- **load**：在接收到完整的响应数据时触发。

- **loadend**：在通信完成或者触发 error、abort 或 load 事件后触发。

  

  每个请求都从触发 loadstart 事件开始，接下来是一或多个 progress 事件，然后触发 error、

abort 或 load 事件中的一个，最后以触发 loadend 事件结束。

```js
var xhr = createXHR(); 

//load事件
xhr.onload = function(){ 
 if ((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304){ 
 alert(xhr.responseText); 
 } else { 
 alert("Request was unsuccessful: " + xhr.status); 
 } 
}; 

//progress事件
xhr.onprogress = function(event){ 
 var divStatus = document.getElementById("status"); 
 if (event.lengthComputable){ 
 divStatus.innerHTML = "Received " + event.position + " of " + 
 event.totalSize +" bytes"; 
 } 
};

//error事件
xhr.onerror = function(){ 
 alert("An error occurred."); 
};

xhr.open("get", "altevents.php", true); 
xhr.send(null);
```



## setRequestHeader()

在AJAX中，如果需要像 HTML 表单那样 POST 数据，需要使用 **setRequestHeader()** 方法来添加 HTTP 头。

然后在 send() 方法中规定需要希望发送的数据：

setRequestHeader()方法需要在open()和send()方法之间调用。

```js
xmlhttp.setRequestHeader(header,value);
```



- 发送**json格式**数据，用来告诉服务端消息主体是序列化后的 JSON 字符串。

```js
xmlhttp.setRequestHeader("Content-type","application/json;charset=utf-8");
```

- 发送**表单**数据

```js
xmlhttp.setRequestHeader("Content-type","multipart/form-data;charset=utf-8");
```

- 发送**表单**文件

```js
xmlhttp.setRequestHeader("Content-type","application/x-www-form-urlencoded;charset=utf-8")
```

- 发送**纯文本**,不指定Content-type时，此是默认值值

```js
xmlhttp.setRequestHeader("Content-type","text/plain;charset=utf-8");
```

- 发送**html**文本

```js
xmlhttp.setRequestHeader("Content-type","text/html;charset=utf-8");
```



## 带凭据的请求

​	默认情况下，跨源请求不提供凭据（**cookie、HTTP 认证及客户端 SSL 证明 等 **）。 通 过 将

**withCredentials** 属性设置为 true，可以指定某个请求应该发送凭据。如果服务器接受带凭据的请

求，会用下面的 HTTP 头部来响应。

​	``Access-Control-Allow-Credentials: true ``

​	如果发送的是带凭据的请求，但服务器的响应中没有包含这个头部，那么浏览器就不会把响应交给

JavaScript（于是，responseText 中将是空字符串，status 的值为 0，而且会调用 onerror()事件处

理程序）。



## 请求

### GET请求

GET 是最常见的请求类型，最常用于向服务器查询某些信息。

**XHR**

```js
//初始化请求
xhr.open("get", url, false);
```



### POST请求

使用频率仅次于 GET 的是 POST 请求，通常用于向服务器发送应该被保存的数据。

**XHR**

```js
xhr.open("post",url, true);
```

### PUT（替换资源） 

### DELETE（删除资源） 

### OPTIONS （允许客户端查看服务器性能） 

### TRACE（回显服务器收到的请求，用于测试或诊断）



# FormData #

1、将表单对象中的数据拼接成请求参数的模式
2、可以实现异步上传**二进制文件**，如图片文件，视频文件，音频文件

```js
var formData = new FormData();

formData.append("username", "Groucho");
formData.append("accountnum", 123456); // 数字 123456 会被立即转换成字符串 "123456"

```



用formdata上传图片


```js
var formdata=new FormData(fromId("advForm"));       //将上传的头像转为formdata形式

    axios({
        method:'post',
        url:'http://47.97.204.234:3000/user/alterAvatar',
        data:formdata,
        cache: false,            // 不缓存 
        contentType: false,  // 不设置内容类型  jQuery不要去设置Content-Type请求头
        processData: false,  // jQuery不要去处理发送的数据
        withCredentials:true,	//携带cookie
    })
    .then(function(resp){
        alert(resp.data.result + resp.data.message);
        toPersonalHomepage();
    })
    .catch(err => console.error(err));
```



# XHR、JQ和AXIOS实现请求

## XHR

```js
var xmlhttp;
xmlhttp =new XMLHttpRequest();	//请求
xmlhttp.open("GET","URL",true);		//发送请求,true代表异步
xmlhttp.send();		//发送数据JSON&text
xmlhttp.setRequestHeader(header,value); 		//请求头
```

> 具体在上面



## JQ

```js
$.ajax({    
     url : "URL",		//网站    
     type : "POST",		//方式    
     data : $( '#postForm').serialize(),    	//传数据
     success : function(data) {    	//请求成功
          $( '#serverResponse').html(data);    
     },    
     error : function(data) {    	//请求失败
          $( '#serverResponse').html(data.status + " : " + data.statusText + " : " + data.responseText);    
     }    
});   
```

### 主要参数

- **type**

类型：String

默认值: "GET")。请求方式 ("POST" 或 "GET")， 默认为 "GET"。注意：其它 HTTP 请求方法，如 PUT 和 DELETE 也可以使用，但仅部分浏览器支持。

- **url**

类型：String

默认值: 当前页地址。发送请求的地址。

- **data**

类型：String

发送到服务器的数据。将自动转换为请求字符串格式。GET 请求中将附加在 URL 后。查看 processData 选项说明以禁止此自动转换。必须为 Key/Value 格式。如果为数组，jQuery 将自动为不同值对应同一个名称。如 {foo:["bar1", "bar2"]} 转换为 '&foo=bar1&foo=bar2'。

- **contentType**

类型：String

默认值: "application/x-www-form-urlencoded"。发送信息至服务器时内容编码类型。

默认值适合大多数情况。如果你明确地传递了一个 ``content-type`` 给 ``$.ajax()`` 那么它必定会发送给服务器（即使没有数据要发送）。

- **error**

类型：Function

默认值: 自动判断 (xml 或 html)。请求失败时调用此函数。

有以下三个参数：XMLHttpRequest 对象、错误信息、（可选）捕获的异常对象。

如果发生了错误，错误信息（第二个参数）除了得到 null 之外，还可能是 "timeout", "error", "notmodified" 和 "parsererror"。

这是一个 Ajax 事件。

- **success**

类型：Function

请求成功后的回调函数。

参数：由服务器返回，并根据 dataType 参数进行处理后的数据；描述状态的字符串。

这是一个 Ajax 事件。

- **dataType**

类型：String

预期服务器返回的数据类型。如果不指定，jQuery 将自动根据 HTTP 包 MIME 信息来智能判断，比如 XML MIME 类型就被识别为 XML。在 1.4 中，JSON 就会生成一个 JavaScript 对象，而 script 则会执行这个脚本。随后服务器端返回的数据会根据这个值解析后，传递给回调函数。可用值:

```text
"xml": 返回 XML 文档，可用 jQuery 处理。
"html": 返回纯文本 HTML 信息；包含的 script 标签会在插入 dom 时执行。
"script": 返回纯文本 JavaScript 代码。不会自动缓存结果。除非设置了 "cache" 参数。注意：在远程请求时(不在同一个域下)，所有 POST 请求都将转为 GET 请求。（因为将使用 DOM 的 script标签来加载）
"json": 返回 JSON 数据 。
"jsonp": JSONP 格式。使用 JSONP 形式调用函数时，如 "myurl?callback=?" jQuery 将自动替换 ? 为正确的函数名，以执行回调函数。
"text": 返回纯文本字符串
```

### 其他参数

- **options**

类型：Object

可选。AJAX 请求设置。所有选项都是可选的。

- **async**

类型：Boolean

默认值: true。默认设置下，所有请求均为异步请求。如果需要发送同步请求，请将此选项设置为 false。

注意，同步请求将锁住浏览器，用户其它操作必须等待请求完成才可以执行。

- **beforeSend(XHR)**

类型：Function

发送请求前可修改 XMLHttpRequest 对象的函数，如添加自定义 HTTP 头。

XMLHttpRequest 对象是唯一的参数。

这是一个 Ajax 事件。如果返回 false 可以取消本次 ajax 请求。

- **cache**

类型：Boolean

默认值: true，dataType 为 script 和 jsonp 时默认为 false。设置为 false 将不缓存此页面。

- **complete(XHR, TS)**

类型：Function

请求完成后回调函数 (请求成功或失败之后均调用)。

参数： XMLHttpRequest 对象和一个描述请求类型的字符串。

这是一个 Ajax 事件。

- **context**

类型：Object

这个对象用于设置 Ajax 相关回调函数的上下文。也就是说，让回调函数内 this 指向这个对象（如果不设定这个参数，那么 this 就指向调用本次 AJAX 请求时传递的 options 参数）。比如指定一个 DOM 元素作为 context 参数，这样就设置了 success 回调函数的上下文为这个 DOM 元素。

就像这样：

```js
$.ajax({ url: "test.html", context: document.body, success: function(){
        $(this).addClass("done");
      }});
```

- **dataFilter**

类型：Function

给 Ajax 返回的原始数据的进行预处理的函数。提供 data 和 type 两个参数：data 是 Ajax 返回的原始数据，type 是调用 jQuery.ajax 时提供的 dataType 参数。函数返回的值将由 jQuery 进一步处理。

- **global**

类型：Boolean

是否触发全局 AJAX 事件。默认值: true。设置为 false 将不会触发全局 AJAX 事件，如 ajaxStart 或 ajaxStop 可用于控制不同的 Ajax 事件。

- **ifModified**

类型：Boolean

仅在服务器数据改变时获取新数据。默认值: false。使用 HTTP 包 Last-Modified 头信息判断。在 jQuery 1.4 中，它也会检查服务器指定的 'etag' 来确定数据没有被修改过。

- **jsonp**

类型：String

在一个 jsonp 请求中重写回调函数的名字。这个值用来替代在 "callback=?" 这种 GET 或 POST 请求中 URL 参数里的 "callback" 部分，比如 {jsonp:'onJsonPLoad'} 会导致将 "onJsonPLoad=?" 传给服务器。

- **jsonpCallback**

类型：String

为 jsonp 请求指定一个回调函数名。这个值将用来取代 jQuery 自动生成的随机函数名。这主要用来让 jQuery 生成度独特的函数名，这样管理请求更容易，也能方便地提供回调函数和错误处理。你也可以在想让浏览器缓存 GET 请求的时候，指定这个回调函数名。

- **password**

类型：String

用于响应 HTTP 访问认证请求的密码

- **processData**

类型：Boolean

默认值: true。默认情况下，通过data选项传递进来的数据，如果是一个对象(技术上讲只要不是字符串)，都会处理转化成一个查询字符串，以配合默认内容类型 "application/x-www-form-urlencoded"。如果要发送 DOM 树信息或其它不希望转换的信息，请设置为 false。

- **scriptCharset**

类型：String

只有当请求时 dataType 为 "jsonp" 或 "script"，并且 type 是 "GET" 才会用于强制修改 charset。通常只在本地和远程的内容编码不同时使用。

- **traditional**

类型：Boolean

如果你想要用传统的方式来序列化数据，那么就设置为 true。请参考工具分类下面的 jQuery.param 方法。

- **timeout**

类型：Number

设置请求超时时间（毫秒）。此设置将覆盖全局设置。

- **username**

类型：String

用于响应 HTTP 访问认证请求的用户名。

- **xhr**

类型：Function

需要返回一个 XMLHttpRequest 对象。默认在 IE 下是 ActiveXObject 而其他情况下是 XMLHttpRequest 。用于重写或者提供一个增强的 XMLHttpRequest 对象。



### 回调函数

如果要处理 $.ajax() 得到的数据，则需要使用回调函数：**beforeSend、error、dataFilter、success、complete。**

### beforeSend

在发送请求之前调用，并且传入一个 XMLHttpRequest 作为参数。

### error

在请求出错时调用。传入 XMLHttpRequest 对象，描述错误类型的字符串以及一个异常对象（如果有的话）

### dataFilter

在请求成功之后调用。传入返回的数据以及 "dataType" 参数的值。并且必须返回新的数据（可能是处理过的）传递给 success 回调函数。

### success

当请求之后调用。传入返回后的数据，以及包含成功代码的字符串。

### complete

当请求完成之后调用这个函数，无论成功或失败。传入 XMLHttpRequest 对象，以及一个包含成功或错误代码的字符串。



# axios #

[详情见]: https://www.kancloud.cn/yunye/axios/234845

Axios 是一个基于 promise 的 HTTP 库，可以用在浏览器和 node.js 中。

安装

```html
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
```

小例

```js
axios
    .post( UILbase + "/user/login", {
        username: userName.value,   //输入框内容（账号）
        password: userPassword.value    //输入框内容 （密码）
    },{
    	withCredentials:true		//携带cookie
	})
    .then(res => loginRequest_status(res))      //调用loginRequest_status 已检查登录是否成功
    .catch(err => console.error(err));      //错误，报错
```

别名

**axios.request(config)**

**axios.get(url[, config])**

**axios.delete(url[, config])**

**axios.head(url[, config])**

**axios.post(url[, data[, config]])**

**axios.put(url[, data[, config]])**

**axios.patch(url[, data[, config]])**

可以使用自定义配置新建一个 axios 实例

**axios.create([config])**

```js
var instance = axios.create({
  baseURL: 'https://some-domain.com/api/',
  timeout: 1000,
  headers: {'X-Custom-Header': 'foobar'}
});

instance({
    url:'/uesr/1234',
    method: 'get'
}) .then(function (response) {
    console.log(response);
  })
```



### request

```js
axios.request({
   method:'post',
   url:'/uesr/1234',
   data:{
   		firstName: 'Fred',
    	lastName: 'Flintstone'
   }
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
```



### 执行 `GET` 请求

```js
// 为给定 ID 的 user 创建请求
axios.get('/user?ID=12345')
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });

// 可选地，上面的请求可以这样做
axios.get('/user', {
    params: {
      ID: 12345
    }
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
```



### 执行 `POST` 请求

```js
axios.post('/user', {
    firstName: 'Fred',
    lastName: 'Flintstone'
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
```



### 执行多个并发请求

```js
function getUserAccount() {
  return axios.get('/user/12345');
}

function getUserPermissions() {
  return axios.get('/user/12345/permissions');
}

axios.all([getUserAccount(), getUserPermissions()])
  .then(axios.spread(function (acct, perms) {
    // 两个请求现在都执行完成
  }));

//----------
// 批量请求数据

function getData() {
  axios
    .all([
      axios.get("http://jsonplaceholder.typicode.com/todos?_limit=5"),
      axios.get("http://jsonplaceholder.typicode.com/posts?_limit=5")
    ])
    // .then(res => showOutput(res[1]))
    .then(axios.spread((todos, posts) => showOutput(posts)))
    .catch(err => console.error(err));
}

```



### 请求参数

这些是创建请求时可以用的配置选项。只有 `url` 是**必需的**。如果没有指定 `method`，请求将默认使用 `get` 方法。

```js
{
  // `url` 是用于请求的服务器 URL
  url: '/user',

  // `method` 是创建请求时使用的方法
  method: 'get', // 默认是 get

  // `baseURL` 将自动加在 `url` 前面，除非 `url` 是一个绝对 URL。
  // 它可以通过设置一个 `baseURL` 便于为 axios 实例的方法传递相对 URL
  baseURL: 'https://some-domain.com/api/',

  // `transformRequest` 允许在向服务器发送前，修改请求数据
  // 只能用在 'PUT', 'POST' 和 'PATCH' 这几个请求方法
  // 后面数组中的函数必须返回一个字符串，或 ArrayBuffer，或 Stream
  transformRequest: [function (data) {
    // 对 data 进行任意转换处理

    return data;
  }],

  // `transformResponse` 在传递给 then/catch 前，允许修改响应数据
  transformResponse: [function (data) {
    // 对 data 进行任意转换处理

    return data;
  }],

  // `headers` 是即将被发送的自定义请求头
  headers: {'X-Requested-With': 'XMLHttpRequest'},

  // `params` 是即将与请求一起发送的 URL 参数
  // 必须是一个无格式对象(plain object)或 URLSearchParams 对象
  params: {
    ID: 12345
  },

  // `paramsSerializer` 是一个负责 `params` 序列化的函数
  // (e.g. https://www.npmjs.com/package/qs, http://api.jquery.com/jquery.param/)
  paramsSerializer: function(params) {
    return Qs.stringify(params, {arrayFormat: 'brackets'})
  },

  // `data` 是作为请求主体被发送的数据
  // 只适用于这些请求方法 'PUT', 'POST', 和 'PATCH'
  // 在没有设置 `transformRequest` 时，必须是以下类型之一：
  // - string, plain object, ArrayBuffer, ArrayBufferView, URLSearchParams
  // - 浏览器专属：FormData, File, Blob
  // - Node 专属： Stream
  data: {
    firstName: 'Fred'
  },

  // `timeout` 指定请求超时的毫秒数(0 表示无超时时间)
  // 如果请求话费了超过 `timeout` 的时间，请求将被中断
  timeout: 1000,

  // `withCredentials` 表示跨域请求时是否需要使用凭证
  withCredentials: false, // 默认的

  // `adapter` 允许自定义处理请求，以使测试更轻松
  // 返回一个 promise 并应用一个有效的响应 (查阅 [response docs](#response-api)).
  adapter: function (config) {
    /* ... */
  },

  // `auth` 表示应该使用 HTTP 基础验证，并提供凭据
  // 这将设置一个 `Authorization` 头，覆写掉现有的任意使用 `headers` 设置的自定义 `Authorization`头
  auth: {
    username: 'janedoe',
    password: 's00pers3cret'
  },

  // `responseType` 表示服务器响应的数据类型，可以是 'arraybuffer', 'blob', 'document', 'json', 'text', 'stream'
  responseType: 'json', // 默认的

  // `xsrfCookieName` 是用作 xsrf token 的值的cookie的名称
  xsrfCookieName: 'XSRF-TOKEN', // default

  // `xsrfHeaderName` 是承载 xsrf token 的值的 HTTP 头的名称
  xsrfHeaderName: 'X-XSRF-TOKEN', // 默认的

  // `onUploadProgress` 允许为上传处理进度事件
  onUploadProgress: function (progressEvent) {
    // 对原生进度事件的处理
  },

  // `onDownloadProgress` 允许为下载处理进度事件
  onDownloadProgress: function (progressEvent) {
    // 对原生进度事件的处理
  },

  // `maxContentLength` 定义允许的响应内容的最大尺寸
  maxContentLength: 2000,

  // `validateStatus` 定义对于给定的HTTP 响应状态码是 resolve 或 reject  promise 。如果 `validateStatus` 返回 `true` (或者设置为 `null` 或 `undefined`)，promise 将被 resolve; 否则，promise 将被 rejecte
  validateStatus: function (status) {
    return status >= 200 && status < 300; // 默认的
  },

  // `maxRedirects` 定义在 node.js 中 follow 的最大重定向数目
  // 如果设置为0，将不会 follow 任何重定向
  maxRedirects: 5, // 默认的

  // `httpAgent` 和 `httpsAgent` 分别在 node.js 中用于定义在执行 http 和 https 时使用的自定义代理。允许像这样配置选项：
  // `keepAlive` 默认没有启用
  httpAgent: new http.Agent({ keepAlive: true }),
  httpsAgent: new https.Agent({ keepAlive: true }),

  // 'proxy' 定义代理服务器的主机名称和端口
  // `auth` 表示 HTTP 基础验证应当用于连接代理，并提供凭据
  // 这将会设置一个 `Proxy-Authorization` 头，覆写掉已有的通过使用 `header` 设置的自定义 `Proxy-Authorization` 头。
  proxy: {
    host: '127.0.0.1',
    port: 9000,
    auth: : {
      username: 'mikeymike',
      password: 'rapunz3l'
    }
  },

  // `cancelToken` 指定用于取消请求的 cancel token
  // （查看后面的 Cancellation 这节了解更多）
  cancelToken: new CancelToken(function (cancel) {
  })
}
```



### 拦截器

在请求或响应被 `then` 或 `catch` 处理前拦截它们。

有时我们需要配置一些全局参数，比如token啦，设置超时时间啦，未登录状态踢出啦等等。又或者加入动画。
这些参数的设置，当然不可能一个一个请求加了，否则累的吐血也不一定能达到目的，最好的办法就是通过拦截器让每个请求都可以加上配置参数。

```js
// 添加请求拦截器
axios.interceptors.request.use(function (config) {
    // 在发送请求之前做些什么
    return config;
  }, function (error) {
    // 对请求错误做些什么
    return Promise.reject(error);
  });

// 添加响应拦截器
axios.interceptors.response.use(function (response) {
    // 对响应数据做点什么
    return response;
  }, function (error) {
    // 对响应错误做点什么
    return Promise.reject(error);
  });
```

![啊啦啦啦](https://upload-images.jianshu.io/upload_images/6522842-afdd970aecd14d01.png?imageMogr2/auto-orient/strip|imageView2/2/w/514/format/webp)

如果你想在稍后移除拦截器，可以这样：

```js
var myInterceptor = axios.interceptors.request.use(function () {/*...*/});
axios.interceptors.request.eject(myInterceptor);
```

可以为自定义 axios 实例添加拦截器

```js
var instance = axios.create();

instance.interceptors.request.use(function () {/*...*/});
```



### 取消请求

使用 *cancel token* 取消请求

> Axios 的 cancel token API 基于[cancelable promises proposal](https://github.com/tc39/proposal-cancelable-promises)，它还处于第一阶段。

可以使用 `CancelToken.source` 工厂方法创建 cancel token，像这样：

```js
var CancelToken = axios.CancelToken;
var source = CancelToken.source();

axios.get('/user/12345', {
  cancelToken: source.token
}).catch(function(thrown) {
  if (axios.isCancel(thrown)) {
    console.log('Request canceled', thrown.message);
  } else {
    // 处理错误
  }
});

// 取消请求（message 参数是可选的）
source.cancel('Operation canceled by the user.');
```

还可以通过传递一个 executor 函数到 `CancelToken` 的构造函数来创建 cancel token：

```js
var CancelToken = axios.CancelToken;
var cancel;

axios.get('/user/12345', {
  cancelToken: new CancelToken(function executor(c) {
    // executor 函数接收一个 cancel 函数作为参数
    cancel = c;
  })
});

// 取消请求
cancel();
```

Note : 可以使用同一个 cancel token 取消多个请求

在请求前取消，**解决短时间内多次点击同一个按钮发送请求会加重服务器的负担**



# 三种请求方式的优异

## XHR

**优点**：

1. 不重新加载页面的情况下更新网页
2. 在页面已加载后从服务器请求/接收数据
3. 在后台向服务器发送数据。

**缺点**：

1. 使用起来也比较繁琐，需要设置很多值。
2. 早期的IE浏览器有自己的实现，这样需要写兼容代码。



##  jQuery ajax

**优点**：

1. 对原生`XHR`的封装，做了兼容处理，简化了使用。
2. 增加了对`JSONP`的支持，可以简单处理部分跨域。

**缺点**：

1. 如果有多个请求，并且有依赖关系的话，容易形成**回调地狱**。
2. 本身是针对**MVC**的编程，不符合现在前端**MVVM**的浪潮。
3. ajax是jQuery中的一个方法。如果只是要使用ajax却要引入整个jQuery非常的不合理。



## axios

**优点**：

1. 从浏览器中创建`XMLHttpRequests`
2. 从 `node.js` 创建 `http` 请求
3. 支持 `Promise` API
4. 拦截请求和响应（就是有interceptor）
5. 转换请求数据和响应数据
6. 取消请求
7. 自动转换 `JSON` 数据
8. 客户端支持防御 `XSRF`

**缺点**：

1. 只持现代代浏览器.



## fetch

还有一种请求方法（先做了解）

```
fetch('/users.json', {
    method: 'post', 
    mode: 'no-cors',
    data: {}
}).then(function() { /* handle response */ });
```



