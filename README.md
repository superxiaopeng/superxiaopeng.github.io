# superxiaopeng.github.io
A website for testing
# 使用JavaScript实现Ajax类

``` javascript
const xhr = new XMLHttpRequest();
```
使用new关键字调用函数，会做下面五件事情：
1、创建一个空对象({}，就是我们所说的实例对象)
2、
3、把该实例对象绑定到this上
4、在函数内部，通过在this上添加属性和方法，来写入实例对象内部()
5、函数默认返回该实例对象
然后把实例对象保存在xhr变量里，在函数外部可以通过xhr变量来为实例对象写入属性和方法(别忘了在函数内部可以使用this做这件事)，就像下面这样：
`xhr.name = 'xiaopeng'; //本行代码只做示例用，不会包裹在主代码里`
上面的代码向我们演示了如何在实例对象内部添加一个键为name，值为'xiaopeng'的属性。

###  实现HTTP POST请求
 我们使用XMLHttpRequest对象提供的open、setRequestHeader及send方法来实现一个post方法，就像下面这样：
``` javascript
	//post()发送HTTP POST请求
	function post() {
		xhr.open('POST', '/hello', true);
		xhr.setRequestHeader({'Content-Type': 'application/json'});
		xhr.send(JSON.stringify({name: 'xiaopeng'}));
	}
``` 
上面通过xhr来访问方法(别忘了xhr变量里还保存着XMLHttpRequest对象的实例对象，在创建实例对象的过程中，open、setrequestHeader及send方法会被拷贝一份，然后放入实例对象里)
### 处理浏览器接收到数据
实现一个receiveData()来做这项工作：
``` javascript
	//接收服务器传来的数据
	function receiveData() {
		//onreadystatechange属性监听readyState值的变化
		xhr.onreadystatechange = function() {
			if(xhr.readyState === 4) {
				if(xhr.status === 200) {
					/*
					只有readyState值为4且status值为200时，
					才表明浏览器已经接收到服务器发来的全部数据。
					response属性的值就是接收到的数据
					*/
					console.log(xhr.response);
				} else {
					console.log('出错了')；
				}
			} else {
				console.log(readyState);
			}
		}
	}
```

`readyState`属性有五个值

| 值 | 描述 |
| --- | ---- |
| 0 | XMLHttpRequest对象已被实例化 |
| 1 | 已调用open，未调用send(HTTP请求还没有发送给服务器) |
| 2 | 已调用send(HTTP请求已被发送给服务器，但未接收到响应数据) | 
| 3 | 所有响应头部都已被接收到，响应体开始被接收但未完成 
| 4 | 所有HTTP响应数据都已被接收到 |

`onreadystatechange`属性指定的函数会在readyState发生改变时被调用，通常`onreadystatechange`要设置在`open`方法之前，`XMLHttpRequest`对象被实例化(`new XMLHttpRequest()`)之后，就像下面这样：
``` javascript
	const xhr = new XMLHttpRequest();
	xhr.onreadystatechange = function() {
		if(xhr.readyState === 4) {
			if(xhr.status === 200) {
				
				console.log(xhr.response);
			} else {
				console.log('出错了')；
			}
		} else {
			console.log(readyState);
		}
	}
	xhr.open();
	//...
```
`onreadystatechange`属性指定的函数会被调用四次，在`new XMLHttpRequest()`之后，`readyState`值为0(回顾一下上面的表格)，在完成一次完整的请求与响应的过程之后，`readyState`值为4， `readyState`改变了四次，所以函数也被调用了四次。

我们来回顾一下到目前为止的所有代码：
``` javascript
	const xhr = new XMLHttpRequest();
	//post()实现HTTP POST请求
	function post() {
		//onreadystatechange通常设置在open之前
		receiveData();
		
		xhr.open('POST', '/hello', true);
		xhr.setRequestHeader({'Content-Type': 'application/json'});
		xhr.send(JSON.stringify({name: 'xiaopeng'}));
	}
	//接收服务器传来的数据
	function receiveData() {
		//onreadystatechange属性监听readyState值的变化
		xhr.onreadystatechange = function() {
			if(xhr.readyState === 4) {
				if(xhr.status === 200) {
					/*
					只有readyState值为4且status值为200时，
					才表明浏览器已经接收到服务器发来的全部数据。
					response属性的值就是接收到的数据
					*/
					console.log(xhr.response);
				} else {
					console.log('出错了');
				}
				
			} else {
				console.log(xhr.readyState);
			}
		}
	}
```
你应该这样向服务器发送`HTTP POST`请求：
``` javascript
	post();
```
### 实现HTTP GET请求
类似于`post`方法的实现，定义一个`get`方法：
``` javascript
	//get()发送HTTP GET请求
	function get() {
		//onreadystatechange通常设置在open之前
		receiveData();
		xhr.open('GET', '/hello', true);
		xhr.send();
	}
```
`GET`请求不需要向服务器提交数据，可以不需要`setRequestHeader`来指定提交的数据类型。
### 实现Ajax类
接下来我们使用ES6的class关键字把上述所有代码封装进一个名为`Ajax`的类里面，就像下面这样：
``` javascript
	
```




