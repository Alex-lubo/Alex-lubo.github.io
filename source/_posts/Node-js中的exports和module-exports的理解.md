---
title: Node.js中的exports和module.exports的理解
date: 2018-04-26 21:58:57
tags:
---

Javascript采用Module来把相关的代码封装到一起，然后解释执行时可以很方便地将这些代码组装成一个文件中运行。而Node.js写模块时，只需要关注require，exports，module这几个变量就可以了，通过把不同的功能写成独立模块，这样可以提高模块的可读性，减少模块的耦合。

按照[官方文档](https://nodejs.org/docs/latest/api/modules.html#modules_exports_shortcut)的说明。
> The exports variable is available within a module's file-level scope, and is assigned the value of module.exports before the module is evaluated.

来说就是如下等式是成立的：
```
exports = module.exports
```
module.exports.f = {...}可以简洁的标示exports.f = {...} 。也就是说**exports是module.exports的引用**。
只是需要注意，在模块被解释调用之前，将module.exports赋值给exports，如果之后给exports赋新值，不会改变module.exports的值。如
```
// test.js
module.exports = {hello: true}; // 导出对象，可以被引用
exports = {hello: false}; // 改变了exports的引用，与module.exports指向的内容是不一样的，这样与module.exports就没有毛线关系了，因此不能导出，只能文件内被访问。

// main.js
const a = require('./test')
console.log('a----',a); // a---- { hello: true }
```
当module.exports的被重新赋值时，exports也会被重新赋值。

require方能看到的只有module.exports的对象，看不到exports的对象。可以通过exports.XX来修改module.exports.XX，但是不能用exports = {...}来修改module.exports导出的内容。