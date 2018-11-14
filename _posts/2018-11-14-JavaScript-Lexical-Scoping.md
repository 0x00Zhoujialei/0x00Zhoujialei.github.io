---
layout:     post
title:      "JavaScript Lexical Scoping"
subtitle:   "以一小块代码为例讲讲词法作用域是个啥"
date:       2018-11-14
author:     "zhoujialei"
header-img: "img/post-bg-unix-linux.jpg"
tags:
    - 开发
    - 翻译计划
    - front-end
    - vim
---

# JavaScript Lexical Scoping

之前前端课程群里的一位小伙伴被下面的代码困住了

```
var a = {
	name: () => {
		console.log(this)
	}
}
a.name() // 输出什么？this是什么?
```

联想起之前的在coursera上过的[程序设计语言](https://www.coursera.org/learn/programming-languages/home/welcome)有说过与此相关的内容，解题发挥一下。

## scope

> Scope refers to the visibility of variables. ...

简单的引用一下[Practical Perl Programming](https://users.cs.cf.ac.uk/Dave.Marshall/PERL/node52.html)对`scope`的定义。大致来说，`scope`决定了当你的代码在运行时，什么变量是可见的(可引用的)什么变量是不可见的(不可引用的)。

在JavaScript中，有两种类型的`scope`:

* `Local scope`
* `Global scope`

在`JavaScript`中，每个函数都创建一个属于自己的新的`scope`。定义在函数中的`variable`对于这个函数来说是局部的(`become local to the function`)，即他们只能在函数内被访问。举个例子：

```
// code here can NOT use carName

function myFunction() {
    var carName = "Volvo";

    // code here CAN use carName

}

// code here can NOT use carName
```

那定义在函数外的`variable`自然就是全局的了(`a variable declared outside a function, becomes global`)，所有全局变量能够被在其他`scope`中被访问和改变:

```
var carName = "Volvo";

// code here can use carName

function myFunction() {

    // code here can also use carName 

}

// code here can use carName
```

## Lexical Scope & Dynamic Scope

好了，我们现在知道了函数创建并拥有属于自己的`scope`(`function scope`)，`scope`决定了当你的代码运行到特定位置的时候当前变量的可见性。但有一个问题事实上我们并没有回答: 当函数被调用的时候，它的`function scope`由什么决定？由被调用的时候所处的`scope`决定？由定义的时候所处的`scope`决定？，以下面的代码为例

```
var a = 2;

function foo() {
	var a = 3;
	bar();
}

function bar() {
	console.log(a)
}

foo() // 会输出什么？2？3？
```
对于上面的问题，假设函数被调用的时候它的`function scope`由其所处的`scope`决定(`Dynamic Scope`): 那么当`bar()`被执行的时候，`bar函数`处于`foo函数`的`scope`中，`变量a`的值为3，结果应该输出3.

假设函数被调用的时候它的`function scope`由定义的时候所处的`scope`决定(`Lexical Scope`): 那么当`bar()`被执行的时候，`bar函数`处于`global scope`中，此时`变量a`的值为2，结果应该输出2.

我们可以在chrome的DevTools的console中输入上述代码，结果应该为2，即`JavaScript`采用的是`Lexical Scope`。

## Arrow Functions

最后，为了解决文章开头代码中的问题，简单介绍一下箭头函数:

> An arrow function does not have its own this; the this value of the enclosing lexical context is used i.e. Arrow functions follow the normal variable lookup rules. So while searching for this  which is not present in current scope they end up finding this from its enclosing scope . 

引用自[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions), 大意是箭头函数自己本身不持有`this`，而是使用其所处的scope内的`this`值，查找`this`的顺序是从最近的scope起从内到外查找。

基于上面的知识，文章开头的问题就迎刃而解了: `a.name()`调用的是name属性持有的箭头函数，这个函数定义在`global scope`中，所以输出的`this`便是在`global scope`中的`this`，比如在chrome的console中执行上述代码得到的是`Window对象`，在node中执行得到的是`global对象`。

## 结语

解决这类问题的关键在于了解function不仅仅只是它含有的code本身，它还持有了当它定义的时候的当前的`environment`，即:

> ... A function value has two parts: The code and the environment that was current when the function was defined

记住是定义的时候的`environment`并且了解类似于箭头函数对于`this`的特殊处理，文章开头的问题也就不难解决了。

## 参考

* [programming languages, UW, coursera](https://www.coursera.org/learn/programming-languages/home/welcome)
* [Arrow functions, MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
* [Closures, MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)
* [YDKJS, Scope & Closures](https://github.com/getify/You-Dont-Know-JS/blob/master/scope%20%26%20closures/apA.md)
* [Practical Perl Programming](https://users.cs.cf.ac.uk/Dave.Marshall/PERL/node52.html)