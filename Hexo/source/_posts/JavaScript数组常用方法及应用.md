---
title: JavaScript数组常用方法及应用
date: 2017-06-24 11:27:18
tags: [JavaScript]
categories: JavaScript
---

# 迭代方法

ES5定义了5个迭代方法，每个方法都接受两个参数：在每一项运行的函数和运行该函数的作用域对象---影响`this`指向。第二个参数为可选参数，通常不用。   

**语法**： 


```js
arr.iterationMethod(callBackFn(element[, index][, array])[, thisArg])
```

**五个迭代方法如下**：    

| 名字        | 做什么          | 返回值                                        | 影响原数组？ | 应用场景                    |
|:---------- |:------------- |:------------------------------------------- |:------- |:------------------------ |
| every()   | 对数组每一项运行给定函数 | true or false：如果数组每一项运行给定函数都返回true，则返回true | 否      | 判断数组所有项是否满足某一条件，比如大于某个数 |
| filter()  | 同上           | 数组：返回数组中执行给定函数后返回值为true的项组成的数组             | 否      | 将符合某种条件的项过滤出来           |
| forEach() | 同上           | 没有返回值：与使用for循环迭代数组一样，但更方便，注意没法中断           | 否      | 代替for循环进行数组迭代           |
| map()     | 同上           | 数组：返回每次执行给：定函数的结果构成的数组，长度和原数组一样            | 否      | 对数组中的项做统一处理             |
| some()    | 同上           | true or false：数组中任一项执行给定函数返回true，则返回true   | 否      | 数组中是否有满足某一条件的项          |

**应用举例**：

- every()

```js
var arr = [1, 2, 3, 4];
var isAllBig = function (element) {
	return element >= 2;
};
arr.every(isAllBig); // false
```

- filter()

```js
var arr = [1, 2, 3, 4];
var getSmallElement = function (element) {
	return element < 2;
};
arr.filter(getSmallElement); // [1]
```

- forEach()

```js
var arr = [1, 2, 3, 4];
var res = [];
var getAnotherArr = function (element) {
	res.push(element * 2)
};
arr.forEach(getAnotherArr);
console.log(res); // [2, 4, 6, 8]
```

> 注意和使用`map()`达到同样效果的区别。实现同样效果有多种方法，找出最优解

- map()

```js
var arr = [1, 2, 3, 4];
var res = []
var getAnotherArr = function (element) {
	return  element * 2;
};
res = arr.map(getAnotherArr); 
console.log(res); // [2, 4, 6, 8]
```

- some()

```js
var arr = [1, 2, 3, 4];
var isSomeBig = function (element) {
	return element >= 2;
};
arr.some(isSomeBig); // true
```

> 注意和`every()`方法的区别

# 位置方法

