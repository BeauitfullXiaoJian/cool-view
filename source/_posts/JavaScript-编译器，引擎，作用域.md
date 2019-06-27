---
title: JavaScript-编译器，引擎，作用域
date: 2019-06-25 21:44:14
tags: javascript
---

##### 基本概念
通常把JavaScript归类为“动态”或“解释执行”的语音，但实际上它是一门编译语言。在传统的编译语言中，程序的源代码在执行之前都会经历三个步骤：
* **词法分析**
    这个过程会把由字符组成的字符串分解成（对编程语音来说）有意义的代码块，这些代码块称为词法单元；如“var a = 0”会被分解成"var","a","=","0"
* **语法分析**
    这个过程把词法单元（数组）转换成一个由元素逐级嵌套所组成的代表了程序语法结构的树。这个树称为"抽象语法树(AST)",var a = 0的抽象语法树可能会有个一个变量声明的顶级节点，接着是一个变量标识节点（值为a）的子节点,以及一个AssignmentExpression的子节点。AssignmentExpression又有一个数字文字（值是2）的子节点
* **代码生成**
    将抽象语法树转换成可执行代码

##### JavaScript引擎
比起编译过程只有3步的语言的编译器，JavaScript引擎要复杂的多。它需要在语法分析和代码生成阶段有特定的步骤来对运行性能进行优化，包括冗余元素进行优化。
JavaScript引擎的编译过程不是发生在构建之前的（不是直接把代码编译成可执行的代码-打包成应用程序，然后双击运行就好了～），大部分编译都是发生在代码片段要执行的前几微秒。

一个JavaScript代码的运行需要由3个东东协同工作
* **引擎**
从头倒胃负责整个程序的编译及其执行过程
* **编译器**
协助引擎;负责词法，语法分析，代码生成
* **作用域**
协助引擎;负责收集和维护由所有变量组成的一系列查询，并按一套严格的规则确定当前执行的代码有没有这些变量的访问权限

对于代码片段`var a = 1`,编译器会做如下处理：
1. 遇到var a,编译器询问作用域是否已经在当前作用域中是否已经有一个这个名称的变量了，如果有的话，编译器会忽略这个声明，继续编译；否则它会请求作用域在当前作用域中新增这个变量（名称为a）
2. 编译器为引擎生成可以运行的代码，这些代码用来处理 a = 2 这个赋值操作。引擎运行时，先询问作用域，当前作用域中有没有名称为a的变量，如果有引擎就使用这个变量，如果没有那么它会向外层嵌套的作用域进行查询直到找到了或查到了最外层作用域（全局作用域）为止，如果最后找到了那么就将2赋值给找到的a变量,如果最后没有找到，那么引擎抛出一个异常

参考代码

```js
var a = 2;
```

```js
var a;
a = 2;
```

如果直接执行这样的代码你可能会得到一个错误`ReferenceError: a is not defined`,然而有些浏览器如果没有设置严格模式，它可能依然有效(这里我们使用了ES5的严格模式‘use strict’)
```js
'use strict'
 a = 2;
```

注意 not defined 和 undefiend的区别，没有声明直接使用变量为`ReferenceError: xxx is not defined`,只声明没有赋值就会默认为undefined的初始值

```js
'use strict'
var b
console.log(b) // 打印 undefined
a = 2 // 抛出异常 Uncaught ReferenceError: a is not defined
```

**undefined与null值**
> 目前，null和undefined基本是同义的，只有一些细微的差别。
1. null表示"没有对象"，即该处不应该有值。(值 null 特指对象的值未设置。它是 JavaScript 基本类型 之一。)[参考文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/null)
 (1) null可以被转换成数字0而undefined不可以（null会执行类型转换undefined不会）
 ```js
typeof null        // "object" (因为一些以前的原因而不是'null')
typeof undefined   // "undefined"
null === undefined // false
null  == undefined // true
null === null // true
null == null // true
!null //true
isNaN(1 + null) // false
isNaN(1 + undefined) // true
 ```
2. undefined表示"缺少值"，就是此处应该有一个值，但是还没有定义
（1）变量被声明了，但没有赋值时，就等于undefined。
（2) 调用函数时，应该提供的参数没有提供，该参数等于undefined。
（3）对象没有赋值的属性，该属性的值为undefined。
（4）函数没有返回值时，默认返回undefined。
 ```js
 'use strict'
 var a = {}
 console.log(a.title) // undefined

 function testFunc(a){
     console.log(a)
 }
 testFunc() // undefined

 console.log(testFunc()) // undefined
 ```

##### 作用域
....
....
等待完善

    
