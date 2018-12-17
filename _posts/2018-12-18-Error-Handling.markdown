---
layout:     post
title:      "Error Handling"
subtitle:   "FE-star course 5 -- material note"
date:       2018-12-18
author:     "zhoujialei"
header-img: "img/post-bg-js-version.jpg"
tags:
    - 开发
    - material note
    - FE-star
    - The Art of Readable Code
---

记一下课程材料里面的笔记，比较流水账就见谅了啊

# Error Handling

## GlobalEventHandlers.onerror

* An event handler for the `error` event. Error events are fired at various targets for different kinds of errors:
    * When a **JavaScript runtime error**(including syntax errors and exceptions thrown within handlers) occurs, an `error` event using interface `ErrorEvent` is fired at `window` and `window.onerror()` is invoked (as well as handlers attached by `window.addEventListener`)(not only capturing).
    * When a resource(such as an `<img>` or `script`) **fails to load**, an `error` event using interface `Event` is fired at the element that initiated the load, and the `onerror()` handler on the element is invoked. These error events do not bubble up to window, but (at least in Firefox) can be handled with a single capturing `window.addEventListener`.
    * Summary: two ways to write `error` event handler for window, and one way for element:
        * `window.onerror = function(message, source, lineno, colno, error) { ... }`
        * `window.addEventListener('error', function(event) { ... })`
        two above are equal.
        * `element.onerror = function(event) { ... }`
        It is said that element's error events can be captured by window at Firefox.

## Script Error issue

### case

When you have done some work with the JavaScript `onerror` event before, you've probably come across the following:

```
Script error.
```

"Script error" is what browsers send to the **onerror callback** when an error originates from a JavaScript served from a *different origin*(different domain, port, or protocol). It's painful because even though there's an error occurring, you don't know `what` the error is, nor from *which* code it's originating. And that's the whole purpose of `window.onerror` - getting insight into uncaught errors in your application.

Summary: cross-origin issues, browsers intentionally hide errors originating from script files from different origins for security reasons.

### The fix: CORS attributes and headers

* Add a crossorigin="anonymous" script attribute

This tells the browser that the target file should be fetched "anonymously" -- No potentially user-identifying information like cookies or HTTP credentials will be transmitted by the browser to the server when requesting this file.

* Add a Cross Origin HTTP header

```
Access-Control-Allow-Origin: * // most community CDNs set this
```

Above indicating to browsers that any origin can fetch this file, or

```
Access-Control-Allow-Origin: https://www.example.com
```

Above restrict the browser to only a known origin you control.

Then you are done.

### An alternative solution: try/catch

For those code that you don't control resources, using try/catch to wrap them is a surefire way to get insight into errors thrown by cross-origin scripts.

## try...catch...finally

* note finally block is usually used to do clean-up job.
* if the `finally` block returns a value, this value becomes the return value of the entire `try-catch-finally` production, regardless of any `return` statements in the `try` and `catch` blocks. This includes exceptions thrown inside of the catch block.

## unhandledrejection

The `unhandledrejection` event is fired when a JavaScript `Promise` is rejected but there is no rejection handler to deal with the rejection. The `unhandledrejection` event implements the `PromiseRejectionEvent` interface, which inherits from `Event`.

### Examples

#### Simple Reporting

```
window.addEventListener("unhandledrejection", function (event) {
    console.warn("WARNING: unhandled promise rejection. Shame on you! Reason: " + event.reason);
});
```

#### Preventing Default

Many environment report unhandled promise rejections to the console by default. The event is cancelable, and so `preventDefault` prevents that default action:

```
window.addEventListener("unhandledrejection", function (event) {
    // ... your code here to handle the unhandled rejection ...

    // Prevent the default handling (error in console)
    event.preventDefault();
});
```

## Performance

废话不多说了 贴代码

```
// Start with one mark.
performance.mark("mySetTimeout-start");

// Wait some time.
setTimeout(function() {
  // Mark the end of the time.
  performance.mark("mySetTimeout-end");

  // Measure between the two different markers.
  performance.measure(
    "mySetTimeout",
    "mySetTimeout-start",
    "mySetTimeout-end"
  );

  // Get all of the measures out.
  // In this case there is only one.
  var measures = performance.getEntriesByName("mySetTimeout");
  var measure = measures[0];
  console.log("setTimeout milliseconds:", measure.duration)

  // Clean up the stored markers.
  performance.clearMarks();
  performance.clearMeasures();
}, 1000);
```
