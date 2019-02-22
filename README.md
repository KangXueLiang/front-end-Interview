# 每天记录一道前端面试题
## <a name='preface'>前言</a> ##
为了技术的进一步进步及基础的巩固，需要不间断的学习。
## HTML
## CSS
## JS
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
         2、如果字符串 string 以"0"开头, 基数是8（八进制）或者10（十进制），那么具体是哪个基数由实现环境决定。  
	 ECMAScript 5 规定使用10，但是并不是所有的浏览器都遵循这个规定。因此，永远都要明确给出radix参数的值。
         3、如果字符串 string 以其它任何值开头，则基数是10 (十进制)。
         所以以10为基数解析'1'，返回 1

         parseInt('2', 1) 以1为基数解析'2'， 2不是有效的一进制数字，无法解析，返回NaN
         同理
         parseInt('3', 2) 3不是有效的二进制数字，无法解析，返回NaN
	  
     [参考MDN-map()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/map)
      
     [参考MDN-parseIng()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/parseInt)
     
## HTTP、HTTPS
- 讲一下你知道的TCP协议
	
    简单来说，一张图：
  
    ![TCP连接](https://user-images.githubusercontent.com/34148615/53062591-3d846300-34fc-11e9-8d0f-4063d9ff3398.png)
	
  [作为前端你了解多少TCP内容](https://juejin.im/post/5c078058f265da611c26c235)
## algorithm
## others
