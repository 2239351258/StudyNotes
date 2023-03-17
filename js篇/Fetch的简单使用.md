<h1>
    <center>Fetch的简单使用</center>
</h1>

## fetch是web提供的一个可以获取异步资源的api，其返回值的是Promise对象。

- 当接收到一个代表错误的 HTTP 状态码时，从 `fetch()` 返回的 Promise **不会被标记为 reject**，即使响应的 HTTP 状态码是 404 或 500。相反，它会将 Promise 状态标记为 resolve（如果响应的 HTTP 状态码不在 200 - 299 的范围内，则设置 resolve 返回值的 [`ok`](https://developer.mozilla.org/zh-CN/docs/Web/API/Response/ok) 属性为 false），仅当网络故障时或请求被阻止时，才会标记为 reject。

### 传统XHR发送请求写法

```javascript
var xhr = new XMLHttpRequest();
xhr.open('GET', url);
xhr.responseType = 'json';

xhr.onload = function() {
  console.log(xhr.response);
};

xhr.onerror = function() {
  console.log("Oops, error");
};

xhr.send();
```

### 使用Fetch发送请求（GET）
```javascript
fetch(url).then(
      response => {
        console.log('联系服务器成功了');
        return response.json()
      },
      error => {
        console.log('联系服务器失败了', error);
        return new Promise(() => { })
      }
    ).then(
      response => { console.log('获取数据成功了', response); },
      error => { console.log('获取数据失败了', error); }
    )
```

### 使用catch接收错误

```javascript
fetch(url).then(
      response => {
        console.log('联系服务器成功了');
        return response.json()
      }).then(
      response => { console.log('获取数据成功了', response); }
    ).catch(error=>{
    console.log(error)
})
```

### 使用async和await语法糖

```javascript
try {
      const response = await fetch(url)
      const data = await response.json()
      console.log(data)
    } catch (error) {
      console.log('请求出错', error)
    }
```

使用await语法糖需在外面包裹一个async函数

### Fetch语法

```
参数:
input:定义要获取的资源。可能的值是：一个URL或者一个Request对象。
init:可选,是一个对象，参数有：
	method: 请求使用的方法，如 GET、POST。
	headers: 请求的头信息，形式为 Headers 对象或 ByteString。
	body: 请求的 body 信息：可能是一个 Blob、BufferSource、FormData、URLSearchParams 或者USVString 对象。注意 GET 或 HEAD 方法的请求不能包含 body 信息。
	mode: 请求的模式，如 cors、 no-cors 或者 same-origin,默认为no-cors,该模式允许来自 CDN 的脚本、其他域的图片和其他一些跨域资源，但是首先有个前提条件，就是请求的 method 只能是HEAD、GET 或 POST。此外，如果 ServiceWorkers 拦截了这些请求，它不能随意添加或者修改除这些之外 Header 属性。第三，JS 不能访问 Response 对象中的任何属性，这确保了跨域时 ServiceWorkers 的安全和隐私信息泄漏问题。cors模式允许跨域请求,same-origin模式对于跨域的请求，将返回一个 error，这样确保所有的请求遵守同源策略。
	credentials: 请求的 credentials，如 omit、same-origin 或者 include。
	cache:  请求的 cache 模式: default, no-store, reload, no-cache, force-cache, or only-if-cached.
返回值：一个 Promise，resolve 时回传 Response 对象。

fetch(input, init).then(function(response) {  });
```

### 发送post请求的示例:

```javascript
fetch(url, {
  method: "POST",
  headers: {
      "Content-Type": "application/json",
  },
  mode: "cors",
  body: JSON.stringify({
      content: "留言内容"
  })
}).then(
  res=>{
      return res.json()
  },
  rej=>{
      // 返回一个pending的promise，中断promise调用链
      return new Promise(()=>{}))
  }
).then(
	res=>{
        // 获取到数据
        console.log(res)
    }
)
```

