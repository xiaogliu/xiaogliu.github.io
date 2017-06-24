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
arr.forEach(callBackFn(element[, index][, array])[, thisArg])
```

**五个迭代方法对比如下**：    

| 名字        | 做什么          | 返回值                                        | 影响原数组？ | 应用场景                    |
|:---------- |:------------- |:------------------------------------------- |:------- |:------------------------ |
| every()   | 对数组每一项运行给定函数 | true or false：如果数组每一项运行给定函数都返回true，则返回true | 否      | 判断数组所有项是否满足某一条件，比如大于某个数 |
| filter()  | 同上           | 数组：返回数组中执行给定函数后返回值为true的项组成的数组             | 否      | 将符合某种条件的项过滤出来           |
| forEach() | 同上           | 没有返回值：与使用for循环迭代数组一样，但更方便，注意没法中断           | 否      | 代替for循环进行数组迭代           |
| map()     | 同上           | 数组：返回每次执行给：定函数的结果构成的数组，长度和原数组一样            | 否      | 对数组中的项做统一处理             |
| some()    | 同上           | true or false：数组中任一项执行给定函数返回true，则返回true   | 否      | 数组中是否有满足某一条件的项          |

**应用举例**：

## every()

```js
var arr = [1, 2, 3, 4];
var isAllBig = function (element) {
	return element >= 2;
};
arr.every(isAllBig); // false
```

## filter()

```js
var arr = [1, 2, 3, 4];
var getSmallElement = function (element) {
	return element < 2;
};
arr.filter(getSmallElement); // [1]
```

## forEach()

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

## map()

```js
var arr = [1, 2, 3, 4];
var res = []
var getAnotherArr = function (element) {
	return  element * 2;
};
res = arr.map(getAnotherArr); 
console.log(res); // [2, 4, 6, 8]
```

## some()

```js
var arr = [1, 2, 3, 4];
var isSomeBig = function (element) {
	return element >= 2;
};
arr.some(isSomeBig); // true
```

> 注意和`every()`方法的区别

# 位置方法

ES5为数组提供了两个位置方法：`indexOf()`和`lastIndexOf()`。   

**语法**

```js
arr.indexOf(searchElement[, fromIndex])
```

**两个迭代方法对比如下**

| 名字 | 做什么   | 返回值 | 影响原数组？ |
|:----- |:------- |:---- |:------- |
| indexOf() | 从数组开头向后查找提供元素的索引值 | 索引值：第一个符合条件的元素的索引值 | 否 |
| lastIndexOf() | 从数组末尾向前查找提供元素的索引值 | 同上 | 否 |

# 操作方法

操作方法有`concat()`, `slice()`以及**万能的**`splice`方法，三种方法对比如下：   


| 名字 | 做什么   | 返回值 | 影响原数组？ |
|:----- |:------- |:---- |:------- |
| concat() | 拼接数组 | 数组：拼接后的新数组 | 否 |
| slice() | 截取数组中指定的部分 | 数组：截取的数组 | 否 |
| splice() | 入参不同可以实现数组的增、删、改 | 数组：删除的元素组成的数组 | 是 |

**应用举例**

## concat()

语法：

```js
var new_array = old_array.concat(value1[, value2[, ...[, valueN]]])
```

举例：

```js
var arr = [1, 2, 3, 4];
var res = arr.concat(5);
console.log(res); // [1, 2, 3, 4, 5]
```
> 如果不传参，返回元素组的副本

## slice()

语法： 

```js
arr.slice();
arr.slice(begin);
arr.slice(begin, end);
```

如果`slice()`方法中不传参数，返回整个数组；只传入一个参数，返回从此位置至数组末尾的元素组成的数组；如果传入两个参数，则返回**第一个参数（包括）至第二个参数（不包括）之间的元素组成的数组**。

举例： 

```js
var arr = [1, 2, 3, 4];
var res = arr.slice(0);
var res1 = arr.slice(1);
var res2 = arr.slice(1, 2);
console.log(res); // [1, 2, 3, 4]
console.log(res1); // [2, 3, 4]
console.log(res2); // [2]
```

> `slice()`也接受负数参数，在此不作讨论。

## splice()

语法： 

```js
array.splice(start)
array.splice(start, deleteCount) 
array.splice(start, deleteCount, item1, item2, ...)
```

`splice()`方法可以对数组进行增、删、改的操作：

| 操作 | 入参 | 返回 |
|:----- |:------- |:---- |
| 删除 | 两个参数：要删除的第一项的位置和要删除的项数 | 数组：删掉的元素组成的数组 |
| 插入 | 三个参数：起始位置、0（删除0项）、要插入的项 | 数组：空数组（删掉的项） |
| 替换 | 三个参数：其实位置、要删除的项数、要插入的项（**当删除的项数和出入的项数相等时，实现更改的目的**） | 数组：删掉的元素组成的数组 |

splice()应用举例   

删除   

```js
var arr = [1, 2, 3, 4];
var removed = arr.splice(1, 2);
console.log(arr); // [1, 4]
console.log(removed); // [2, 3]
```

> 如果不传参，不对数组进行任何操作；如果只传递一个参数，则以此参数为起始位置（包含），删除数组之后所有的项

插入   

```js
var arr = [1, 2, 3, 4];
var removed = arr.splice(1, 0, 5);
console.log(arr); // [1, 2, 5, 3, 4]
console.log(removed); // []
```

替换

```js
var arr = [1, 2, 3, 4];
var removed = arr.splice(1, 1, 'a');
console.log(arr); // [1, 'a', 3, 4]
console.log(removed); // [2]
```

> 要删除的项没必要和要添加的项相等

# 重排序方法

