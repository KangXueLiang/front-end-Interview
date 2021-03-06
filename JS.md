## JS
- 如何创建一个没有原型的对象?
	 ```js
	 var obj = Object.create(null)
	 
	 o = {}
	 o.__proto__ = null
	 
	 o = {}
	 Object.setPrototypeOf(o, null)
	 
	 ```
- 生成一个[min, max]的随机整数
	```js
	function getRandomIntInclusive(min, max) {
		let min = Math.ceil(min)
		let max = Math.floor(max)
		return Math.floor(Math.random()*(max - min + 1)) + min
	}
	```
	* 解析如下
	Math.random() 生成一个[0, 1) 之间的伪随机浮点数，包括0，但不包括1
	
	Math.random() * min 生成一个[0, min) 之间的伪随机浮点数， 包含0， 不包含min
	
	Math.floor(Math.random() * min) 生成一个[0, min) 之间的伪随机整数， 包含0， 不包含min
	
	Math.floor(Math.random() * (max - min)) + min 生成一个[min, max) 之间的伪随机整数， 包含min， 不包含max
	
	Math.floor(Math.random() * (max - min + 1)) + m 生成一个[min, max + 1) 之间的伪随机整数， 包含min， 不包含max + 1,即可以取到包括max的最大值
	
	因为要生成的是一个整数，但不能保证min, max也是整数， 会出现一些问题，所以要先处理他们，一个向上取整，一个向下取整，就OK 了
	
- 解释一下什么是 Event Loop ？
	 结合段代码来说一下
	 ```js
	console.log(1)
	setTimeout(() => {
		console.log(2)
	}, 0)
	new Promise(resolve => {
		console.log(3)
		resolve()
	}).then(() => {
		console.log(4)
	}).then(() => {
		console.log(5)
	})
	console.log(6)
	 ```
	 答案：1 3 6 4 5 2

	 解释如下：
	 * javascript的设计是单线程的，为了不阻塞主线程，就产生了一个解决方案： event loop
	 * event loop分为两部分：
	 
	  微任务(jobs || microtask)包括 process.nextTick ，promise ，MutationObserver。
	  
	  宏任务(task || macrotask)包括 script ， setTimeout ，setInterval ，setImmediate ，I/O ，UI rendering。
	 * 首先执行同步代码，这属于宏任务。
	 * 当执行完所有同步代码后，执行栈为空，查询是否有异步代码需要执行
	 * 执行所有微任务
	 * 当执行完所有微任务后，如有必要会渲染页面
	 * 然后开始下一轮 Event Loop，执行宏任务中的异步代码，也就是 setTimeout 中的回调函数
	 
	 参考：[这一次，彻底弄懂 JavaScript 执行机制](https://juejin.im/post/59e85eebf265da430d571f89)
- 实现call， apply,  bind

	 首先， 需要考虑以下几点
	* 如果不传第一个参数，那么上下文默认为window
	* 改变this的指向，让原函数可以接受参数并执行
	* 给上下文定义的函数必须唯一
	* 扩展完要把自定义函数删除
	
	 好了，首先来实现call
	 ```js
	 Function.prototype.myCall = function(ctx) {
	 	// ctx如果没有传，默认为window
	 	ctx = ctx || window 
		// 生成一个唯一的fn，避免污染可能已有的fn属性或方法
		let fn = Symbol() 
		// 给context添加一个方法 指向this
		ctx.fn = this 
		// 获取当前参数列表
		let arg = [...arguments].slice(1) 
		// 执行fn
		ctx.fn(...arg) 
		// 删除fn
		delete ctx.fn 
	 }
	 ```
	 apply的实现与call基本一致，只是参数传递一个数组，代码如下。
	 ```js
	 Function.prototype.myApply = function(ctx) {
	 	ctx = ctx || window 
		let fn = Symbol() 
		ctx.fn = this 
		let arg = [...arguments].slice(1) 
		ctx.fn(arg) 
		delete ctx.fn 
	 }
	 ```
	 
	 接下来是bind的实现，首先，分析一下bind的功能：
	 
	 * 执行函数，绑定this
	 * 返回一个函数，这里有两种情况，一种是作为构造函数使用，一种是直接调用。要区别处理
	 	* 直接调用常规操作
		* new 使用的时候，因为无法改变构造函数的this，因此要忽略传入的this
	 * 支持多个参数
	 * 支持柯里化形式传参 fn(1)(2)
	 
	 bind的实现是要基于call， apply的，代码如下
	 ```js
	 Function.prototype.myBind = function(ctx) {
	 	let self = this
		let regs = [...arguments].splice(1)
	 	retutn function F () {
			// 因为返回了一个函数，我们可以 new F()，所以需要判断
			if (this instanceof of F) {
				return new self(...args, ...arguments)
			}
			return this.apply(ctx, regs.concat(...arguments)) 
		}
	 }
	 ```
	 
	 
	 
- ['1', '2', '3'].map(parseInt) what & why ?

		 正确答案是[1, NaN, NaN]
         
		 解析： 
         
         先来看一下map()
         
         Array.prototype.map()会创建一个新数组，其结果是该数组中的每个元素都调用一个提供的函数后返回的结果。
         语法： var new_array = arr.map(function callback(currentValue[, index[, array]]) {
              // Return element for new_array }[, 
              thisArg])  
         它的callback可以接受三个参数，分别是当前元素，当前索引，原数组。
         
         再来看一下parseInt() 
         
         parseInt() 函数解析一个字符串参数，并返回一个指定基数的整数 (数学系统的基础)。
         语法：parseInt(string, radix)
         它接受两个参数，string表示要解析的字符串，如果不是字符串，将会调用该参数的toString()方法
                        radix表示一个介于2和36之间的整数(数学系统的基础)，表示上述字符串的基数。默认值为0，表示采用十进制进行解析。
                        
         现在综合来看这道题，['1', '2', '3'].map(parseInt) === [parseInt('1', 0), parseInt('2', 1), parseInt('3', 2)]

         parseInt('1', 0) MDN上有说，当基数为0的时候，JavaScript 作如下处理
         1、如果字符串 string 以"0x"或者"0X"开头, 则基数是16 (16进制).
         2、如果字符串 string 以"0"开头, 基数是8（八进制）或者10（十进制），那么具体是哪个基数由实现环境决定。  ECMAScript 5 规定使用10，但是并不是所有的浏览器都遵循这个规定。因此，永远都要明确给出radix参数的值。
         3、如果字符串 string 以其它任何值开头，则基数是10 (十进制)。
         所以以10为基数解析'1'，返回 1

         parseInt('2', 1) 以1为基数解析'2'， 2不是有效的一进制数字，无法解析，返回NaN
         同理
         parseInt('3', 2) 3不是有效的二进制数字，无法解析，返回NaN
	  
     [参考MDN-map()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/map)
      
     [参考MDN-parseIng()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/parseInt)
