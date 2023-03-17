### 1. async/await、promise和setTimeout的执行顺序

```javascript
async function async1() {
	console.log('async1 start');
	await async2();
	console.log('asnyc1 end');
}
async function async2() {
	console.log('async2');
}
console.log('script start');
setTimeout(() => {
	console.log('setTimeOut');
}, 0);
async1();
new Promise(function (reslove) {
	console.log('promise1');
	reslove();
}).then(function () {
	console.log('promise2');
})
console.log('script end');
```

<details> 
    <summary>答案：</summary>
&emsp;&emsp;script start<br/>
&emsp;&emsp;async1 start<br/>
&emsp;&emsp;async2<br/>
&emsp;&emsp;promise1<br/>
&emsp;&emsp;script end<br/>
&emsp;&emsp;asnyc1 end<br/>
&emsp;&emsp;promise2<br/>
&emsp;&emsp;setTimeOut<br/>
</details>

##### promise、async/await

首先，`new Promise`是同步的任务，会被放到主进程中去立即执行。而`.then()`函数是异步任务会放到异步队列中去，那什么时候放到异步队列中去呢？当你的`promise`状态结束的时候，就会立即放进异步队列中去了。

**带`async`关键字的函数会返回一个`promise`对象**，如果里面没有`await`，执行起来等同于普通函数；如果没有`await`，`async`函数并没有很厉害。

`await` 关键字要在 `async` 关键字函数的内部，`await` 写在外面会报错；`await`如同他的语意，就是在等待，等待右侧的表达式完成。此时的**`await`会让出线程，阻塞`async`内后续的代码，先去执行`async`外的代码**。等外面的同步代码执行完毕，才会执行里面的后续代码。就算`await`的不是`promise`对象，是一个同步函数，也会等这样操作。  