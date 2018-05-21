---
title: Javascript ES6中的for-of
date: 2018-03-29 22:04:46
tags:
---
### JS中如何遍历数组?
在es5中,可以使用Array内建的方法: Array.forEach
```
myArray.forEach(function (value) {
  console.log(value);
});
```
该方法好处是代码看起来简洁.但也存在着缺陷:
- 不能使用break语句中断循环，
- 不能使用return语句返回到外层函数。

### for - in方法:
```
for(var index in myArray) {
  console.log(myArray[index]);
});
```
需要注意,for-in是为普通对象设计的，不适用于数组遍历。它有以下特点:
- 赋给index的值不是实际的数字，而是字符串“0”、“1”、“2”;
- 作用于数组的for-in循环体除了遍历数组元素外，还会遍历自定义属性。举个例子，如果你的数组中有一个可枚举属性myArray.name，循环将额外执行一次，遍历到名为“name”的索引。就连数组原型链上的属性都能被访问到。
- 在某些情况下，这段代码可能按照随机顺序遍历数组元素

### for - of方法:
```
for(var index of myArray) {
  console.log(myArray[index]);
});
```
这时ES6中最简洁、最直接的遍历数组元素(值)的语法
- 这个方法避开了for-in循环的所有缺陷
- 与forEach()不同的是，它可以正确响应break、continue和return语句

强大的 for - of,不仅支持数组，可以遍历其它的集合;
- 大多数类数组对象，例如DOM NodeList对象
- 支持字符串遍历，它将字符串视为一系列的Unicode字符来进行遍历：
```
for (var chr of "") {
    console.log(chr); 
}
```
- 支持Map和Set对象遍历。Map和set也是Es6中的新增的类型.
- Set对象类似于数组，但是成员的值都是唯一的，没有重复的值。可以用一个数组来创建set对象.具体关于Set可以[参考](http://es6.ruanyifeng.com/#docs/set-map).
```
var setNumber = new Set([1, 2, 3, 4, 1, 2,3]);
// 1, 2, 3, 4
for (var value of setNumber) {
    console.log(value);
} // 1 2 3 4
```
- Map对象稍有不同：内含的数据由键值对组成，所以你需要使用解构（destructuring）来将键值对拆解为两个独立的变量：
```
for (var [key, value] of keyMap) {
    console.log(key + " is: " + value);
}
```