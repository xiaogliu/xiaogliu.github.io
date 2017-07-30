---
title: 遍历JS对象属性
date: 2017-07-30 20:13:23
tags: [JavaScript]
categories: [JavaScript]
---

可以通过`for...in`遍历对象中属性。

# 语法（ES3+）

```js
for (variable in object) { ...
}
```

- `variable`：每次循环都会将对象[可枚举属性](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Enumerability_and_ownership_of_properties)名称复制给该变量
- `object`：被遍历的对象

> 1，在遍历过程中是遍历对象中可枚举属性，这意味着，遍历的属性包括通过原型链继承来的属性（如果有）,；
> 2，上面是ES3的语法，兼容IE9以下浏览器，如果不需兼容IE9以下浏览器，参看这个[stackoverflow 问答](https://stackoverflow.com/questions/7306669/how-to-get-all-properties-values-of-a-javascript-object-without-knowing-the-key)解锁更多方法。

# 举例

## 基本用法

```js
var obj = {a: 1, b: 2, c: 3};

for (var prop in obj) {
	console.log(`obj.${prop} = ${obj[prop]}`);
}

// 输出
// "obj.a = 1"
// "obj.b = 2"
// "obj.c = 3"
```

## 不枚举继承的属性

```js
var triangle = {a: 1, b: 2, c: 3};

var ColoredTriangle = function () {
	this.color = 'red';
};

ColoredTriangle.prototype = triangle;

var obj = new ColoredTriangle();

for (var p in obj) {
	// 不枚举继承来的属性
	if (obj.hasOwnProperty(p)) {
		console.log(`obj.${p} = ${obj[p]}`);
	}
}

// 输出
// "obj.color = red"
```

# 参考资料

【1】[MDN for...in](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...in)   
【2】[How to get all properties values of a Javascript Object (without knowing the keys)?](https://stackoverflow.com/questions/7306669/how-to-get-all-properties-values-of-a-javascript-object-without-knowing-the-key)   
【3】[How do I access properties of a javascript object if I don't know the names?](https://stackoverflow.com/questions/675231/how-do-i-access-properties-of-a-javascript-object-if-i-dont-know-the-names)   
