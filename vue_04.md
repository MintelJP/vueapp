### promise

- 主要解决异步深层嵌套的问题
- promise 提供了简洁的API  使得异步操作更加容易

#### Promise基本使用

```html
 
  <script type="text/javascript">

    var p = new Promise(function(resolve, reject){
      //实现异步任务  setTimeout
      setTimeout(function(){
        var flag = false;
        if(flag) {
          //正常情况
          resolve('hello');
        }else{
          //异常情况
          reject('出错了');
        }
      }, 100);
    }); 
    p.then(function(data){
      console.log(data)
    },function(info){
      console.log(info)
    });
  </script>
```

###  基于Promise发送Ajax请求

```html
 
  <script type="text/javascript">
    function queryData(url) {
 
      var p = new Promise(function(resolve, reject){
        var xhr = new XMLHttpRequest();
        xhr.onreadystatechange = function(){
          if(xhr.readyState != 4) return;
          if(xhr.readyState == 4 && xhr.status == 200) {
       
            resolve(xhr.responseText);
          }else{
        
            reject('服务器错误');
          }
        };
        xhr.open('get', url);
        xhr.send(null);
      });
      return p;
    }

    queryData('http://localhost:3000/data')
      .then(function(data){
        console.log(data)
     
        return queryData('http://localhost:3000/data1');
      })
      .then(function(data){
        console.log(data);
        return queryData('http://localhost:3000/data2');
      })
      .then(function(data){
        console.log(data)
      });
  </script>
```

### Promise  基本API

####  实例方法

##### .then()

- 得到异步任务正确的结果

##### .catch()

- 获取异常信息

##### .finally()

- 成功与否都会执行（不是正式标准） 



#### 静态方法

#####  .all()

- `Promise.all`方法接受一个数组作参数，数组中的对象（p1、p2、p3）均为promise实例（如果不是一个promise，该项会被用`Promise.resolve`转换为一个promise)。它的状态由这三个promise实例决定

#####  .race()

- `Promise.race`方法同样接受一个数组作参数。当p1, p2, p3中有一个实例的状态发生改变（变为`fulfilled`或`rejected`），p的状态就跟着改变。并把第一个改变状态的promise的返回值，传给p的回调函数

​	



###  fetch

- Fetch API是新的ajax解决方案 Fetch会返回Promise
- **fetch不是ajax的进一步封装，而是原生js，没有使用XMLHttpRequest对象**。
- fetch(url, options).then(）



####  fetch API  中的 HTTP  请求

- fetch(url, options).then(）
- HTTP协议，它给我们提供了很多的方法，如POST，GET，DELETE，UPDATE，PATCH和PUT
  - 默认的是 GET 请求
  - 需要在 options 对象中 指定对应的 method       method:请求使用的方法 
  - post 和 普通 请求的时候 需要在options 中 设置  请求头 headers   和  body



###  axios

- 基于promise用于浏览器和node.js的http客户端
- 支持浏览器和node.js
- 支持promise
- 能拦截请求和响应
- 自动转换JSON数据
- 能转换请求和响应数据

#### axios基础用法

- get和 delete请求传递参数
  - 通过传统的url  以 ? 的形式传递参数
  -  restful 形式传递参数 
  - 通过params  形式传递参数 
- post  和 put  请求传递参数
  - 通过选项传递参数
  -  通过 URLSearchParams  传递参数 

```js
 
	axios.get('http://localhost:3000/adata').then(function(ret){  
      console.log(ret)
    })
	axios.get('http://localhost:3000/axios?id=123').then(function(ret){
      console.log(ret.data)
    })
   
    axios.get('http://localhost:3000/axios/123').then(function(ret){
      console.log(ret.data)
    })
	
    axios.get('http://localhost:3000/axios', {
      params: {
        id: 789
      }
    }).then(function(ret){
      console.log(ret.data)
    })
	
    axios.delete('http://localhost:3000/axios', {
      params: {
        id: 111
      }
    }).then(function(ret){
      console.log(ret.data)
    })

    axios.post('http://localhost:3000/axios', {
      uname: 'lisi',
      pwd: 123
    }).then(function(ret){
      console.log(ret.data)
    })
	
    var params = new URLSearchParams();
    params.append('uname', 'zhangsan');
    params.append('pwd', '111');
    axios.post('http://localhost:3000/axios', params).then(function(ret){
      console.log(ret.data)
    })

 	
    axios.put('http://localhost:3000/axios/123', {
      uname: 'lisi',
      pwd: 123
    }).then(function(ret){
      console.log(ret.data)
    })

```

#### axios 全局配置

```js
#  配置公共的请求头 
axios.defaults.baseURL = 'https://api.example.com';
#  配置 超时时间
axios.defaults.timeout = 2500;
#  配置公共的请求头
axios.defaults.headers.common['Authorization'] = AUTH_TOKEN;
# 配置公共的 post 的 Content-Type
axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded';


```

###  async  和 await

- async作为一个关键字放到函数前面
  - 任何一个`async`函数都会隐式返回一个`promise`
- `await`关键字只能在使用`async`定义的函数中使用
  - ​    await后面可以直接跟一个 Promise实例对象
  - ​     await函数不能单独使用
- **async/await 让异步代码看起来、表现起来更像同步代码**



