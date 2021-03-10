---
title: JavaScript 中的 reducer
date: 2021-03-10 19:09:18
tags: [JavaScript]
categories: JavaScript
e_title: reducer-in-js
---

很长一段时间一直对 _reducer_ 不太理解，reduce 英文含义 “减少”，reducer 可以翻译成“缩减器”，但感觉还是不直观。知道今天，看到一个函数，豁然开朗：

```js
const uniqueKey = new Set(obj.reduce((acc, { key }) => [...acc, key], []));
```

这个函数的作用是将一个数组对象元素中的 key 都找出来，然后去重，返回的是 `Set` 类型的数据。

## 数组中的 reducer 方法

上个函数是数组中 `reduce` 用法，

```JS
arr.reduce(callback( accumulator, currentValue, [, index[, array]] ) [, initialValue])
```

Your **reducer** (callback) function's returned value is assigned to the accumulator, whose value is remembered across each iteration throughout the array, and ultimately becomes the final, **single resulting value**.

重要信息：

1. 每次 callback function，即 reducer function 返回值都赋值给了 accumulator
2. accumulator 在每次循环中会记录下来
3. 循环结束，accumulator 作为最终值被返回

关于 initialValue

1. 如果提供 initalValue，第一次循环时，accumulator 值为 initialValue，currentValue 为数组第一个元素（同时 index 为 0）；
2. 如果不提供 initalValue，第一次循环时，accumulator 值为数组第一个元素，currentValue 为数组第二个元素（同时 index 为 1，index 始终为 currentValue 的 index）。

> 如果是空数组，不提供 initialValue 会报错。

至此，可以得出结论：reducer 函数的直观理解是 **将多个值（或者对值可能产生的副作用）通过压缩（减少）处理，返回一个**。

比如，数组中 `reduce` 方法可以理解成：

```bash
数组（多个值）-- reducer 函数 --> 单个值
```

## redux 中的 reducer

redux 中也有 reducer，他的作用也能直观理解成 **将多个值（或者对值可能产生的副作用）通过压缩（减少）处理，返回一个**，reducer 接收 state 以及 action（对 state 产生副作用），然后（压缩 preState 和 action）返回新的 state，简单流程是这样：

```bash
preState + action -- reducer 函数 --> newState
```

参考问题 [Why are Redux's state functions called reducers?](https://stackoverflow.com/questions/34376023/why-are-reduxs-state-functions-called-reducers)

## react hook 中的 useReducer

react hook 有一个 `useReducer`，它可以接收一个 reducer 函数，这里的 reducer 函数和 redux 中含义一直：接收 state 和 action，返回新的 state。

```js
const [state, dispatch] = useReducer(reducer, initialArg, init);
```

## reducer 其他应用

比如数组去重

```js
const uniqueArr = arr.reduce((acc, cur) => {
  console.log(acc, cur, acc.indexOf(cur) === -1);
  return acc.indexOf(cur) === -1 ? acc.concat(cur) : acc;
}, []); // 设置初始值，避免数组为空时报错
```

## 总结

reducer 函数的直观理解是 **将多个值（或者对值可能产生的副作用）通过压缩（减少）处理，返回一个**。
