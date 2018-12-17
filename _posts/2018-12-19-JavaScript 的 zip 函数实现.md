---
layout:     post
title:      "JavaScript 的 zip 函数实现"
subtitle:   "借助Iterator可以获得优雅的实现"
date:       2018-12-19
author:     "zhoujialei"
header-img: "img/post-bg-unix-linux.jpg"
tags:
    - 开发
    - front-end
    - JavaScript
---

python3中有一个自建函数`zip`,它具备以下行为:

```
>>> list(zip([1,2], [3,4]))
[(1, 3), (2, 4)]
>>> list(zip([1,2,3], [3,4]))
[(1, 3), (2, 4)]
>>> list(zip([1,2], [3,4,5]))
[(1, 3), (2, 4)]
```

> zip(*iterables)

> Make an iterator that aggregates elements from each of the iterables.
	
> Returns an iterator of tuples, where the i-th tuple contains the i-th element from each of the argument sequences or iterables. The iterator stops when the shortest input iterable is exhausted. With a single iterable argument, it returns an iterator of 1-tuples. With no arguments, it returns an empty iterator.


`zip`函数接收非零个`iterables`对象作为参数, 返回一个以`tuple`为元素的`iterator`，这个`tuple`有以下特征：第n个`tuple`由各个参数的第n个元素组成，从左到右一一对应。

当然我们的主题不是探讨`python`的`zip`函数, 而是如何在`javaScript`中实现`zip`函数。

# 需求

* `zip`函数返回的元素长度应该等于参数中长度最短的元素
* `zip`函数应该能接收任意个元素
* `zip`函数返回的元素中的顺序应该是与参数从左到右一一对应的

# 方案一

先获取最短长度，然后对元素进行`map`操作:

```
function zip(...args) {
    let minLength = args[0].length;
    let ret = [];
    for (let e of args) {
        if (e.length <= minLength) {
            minLength = e.length;
        }
    }
    for (let idx=0; idx<minLength; idx++) {
        ret[idx] = args.map(e => e[idx]);
    }

    return ret;
}

```

```
zip([1,2],[3,4,5])
(2) [Array(2), Array(2)]0: (2) [1, 3]1: (2) [2, 4]length: 2__proto__: Array(0)
zip([1,2,6],[3,4,5])
(3) [Array(2), Array(2), Array(2)]0: (2) [1, 3]1: (2) [2, 4]2: Array(2)0: 61: 5length: 2__proto__: Array(0)length: 3__proto__: Array(0)
zip([1,2,3],[4,5])
(2) [Array(2), Array(2)]0: (2) [1, 4]1: (2) [2, 5]length: 2__proto__: Array(0)
zip([1,2,3],[4],[5,6])
[Array(3)]0: (3) [1, 4, 5]length: 1__proto__: Array(0)
```

看起来还行，也达到了我们的目的。但是这样的实现还是有它的局限性：如果我们希望返回的元素长度是以参数中最长的元素为基准呢？如果我们可以避免先求取最小(最大)长度再遍历获取结果呢？

# 方案二

*如果希望是以最小长度为准，那么对于索引`i`，任意参数的第`i`个元素都应该存在。*

*如果希望是以最长长度为准，那么对于索引`i`，只要有参数的第`i`个元素存在即可构成一个有效的元素。*

借助`JavaScript`的`Array.prototype[@@iterator]()`方法，我们可以写出下面的代码:

```
function zipHelper(func, args) {
  const iterOrigin = args.map(arr => arr.values())
  let iters = iterOrigin.map(i => i.next());
  ret = []
  while(iters[func](it => !it.done)) {
    ret.push(iters.map(it => it.value));
    iters = iterOrigin.map((i) => i.next());
  }
  return ret;
}

const shortZip = (...args) => zipHelper('every', args);

const longZip = (...args) => zipHelper('some', args);

```

```
shortZip([1,2,3],[4,5])
(2) [Array(2), Array(2)]
0: (2) [1, 4]
1: (2) [2, 5]
length: 2
__proto__: Array(0)
shortZip([1,2,3],[4,5,6,7])
(3) [Array(2), Array(2), Array(2)]
0: (2) [1, 4]
1: (2) [2, 5]
2: (2) [3, 6]
length: 3
__proto__: Array(0)
longZip([1,2,3],[4,5])
(3) [Array(2), Array(2), Array(2)]
0: (2) [1, 4]
1: (2) [2, 5]
2: (2) [3, undefined]
length: 3
__proto__: Array(0)
longZip([1,2,3],[4,5,6,7])
(4) [Array(2), Array(2), Array(2), Array(2)]
0: (2) [1, 4]
1: (2) [2, 5]
2: (2) [3, 6]
3: (2) [undefined, 7]
length: 4
__proto__: Array(0)

```

`Array.prototype.every()`判断数组中所有元素是不是能通过参数内的函数的测试，显然可以用于最小长度的判断。

`Array.prototype.some()`判断数组中是否有元素能通过参数内的函数的测试，显然可以用于最大长度的判断。

方案二比较方案一显然更为优雅，函数式编程的特点也更为突出，风格上也更为出色。
