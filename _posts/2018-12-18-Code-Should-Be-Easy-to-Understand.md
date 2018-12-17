---
layout:     post
title:      "Code Should Be Easy to Understand"
subtitle:   "The Art of Readable Code -- chapter 1读书笔记"
date:       2018-12-08
author:     "zhoujialei"
header-img: "img/post-bg-js-version.jpg"
tags:
    - 开发
    - 读书笔记
    - The Art of Readable Code
---

最近有点忙，加上工作上主要以代码优化为主，所以这周就只抽时间看看书做做积累这样子，但是不得不说，这本书还是写的很精细的，易于实践，值得推荐。不多说了 下面附上第一章的笔记正文


# Code Should Be Easy to Understand

Over the past five years, we have collected hundreds of examples of “bad code” (much of it our own), and analyzed what made it bad, and what principles/techniques were used to make it better. What we noticed is that all of the principles stem from a single theme.

> Code should be easy to understand.

## What Makes Code "Better"?

But a lot of times, it’s a tougher choice. For example, is this code:

```
    return exponent >= 0 ? mantissa * (1 << exponent) : mantissa / (1 << -exponent); 
```

better or worse than:

```
    if (exponent >= 0) {
        return mantissa * (1 << exponent);
    } else {
        return mantissa / (1 << -exponent);
	 }
```	 

The first version is more compact, but the second version is less intimidating. Which criterion is more important? In general, how do you decide which way to code something?

## The Fundamental Theorem of Readability

> Code should be written to minimize the time it would take for someone else to understand it.

And when we say “understand,” we have a very high bar for this word. For someone to *fully understand* your code, they should be able to make changes to it, spot bugs, and understand how it interacts with the rest of your code.

*Who cares if someone else can understand it? I'm the only one using the code!* Even if you’re on a one-man project, it’s worth pursuing this goal. That “someone else” might be *you* six months later, when your own code looks unfamiliar to you. And you never know—someone might join your project, or your “throwaway code” might get reused for another project.

## Is Smaller Always Better?

* Generally speaking, the less code you write to solve a problem, the better
* But fewer lines isn’t always better! There are plenty of times when a one-line expression like:
 
	```
    assert((!(bucket = FindBucket(key))) || !bucket->IsOccupied());
	```

	takes more time to understand than if it were two lines:
	
	```
    bucket = FindBucket(key);
    if (bucket != NULL) assert(!bucket->IsOccupied());
    ```
    
* Similarly, a comment can make you understand the code more quickly, even though it “adds code” to the file:

	```
	    // Fast version of "hash = (65599 * hash) + c"
	    hash = (hash << 6) + (hash << 16) - hash + c;    
	```
	
## Does Time-Till-Understanding Conflict with Other Goals?    

What about other constraints, like making code efficient, or well-architected, or easy to test, and so on? Don’t these sometimes conflict with wanting to make code easy to understand?

We’ve found that these other goals don't interfere much at all. Even in the realm of highly optimized code, there are still ways to make it highly readable as well. And making your code easy to understand often leads to code that is well architected and easy to test.

## The Hard Part

* Yes, it requires extra work to constantly think about whether an imaginary outsider would find your code easy to understand. Doing so requires turning on a part of your brain that might not have been on while coding before.

* But if you adopt this goal (as we have), we're certain you will become a better coder, have fewer bugs, take more pride in your work, and produce code that everyone around you will love to use. So let’s get started!


























