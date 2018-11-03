---
layout:     post
title:      "一门前端课程的第一周预习测试总结"
subtitle:   "或许眼高手低说的就是自己？（头图暂时没时间配，下次下次~~）"
date:       2018-11-04
author:     "zhoujialei"
header-img: "img/post-bg-js-version.jpg"
tags:
    - 开发
    - course
    - front-end
---

报了一门前端课，倒不是被这门课的内容所吸引，只是因为身边做前端的同学寥寥无几，借着这门课多认识一些朋友，大家多交流沟通也不错。by the way，毕竟除了课程内容外，它画的饼看起来味道也不错。

第一周课前预习作业只有三道题的量，内容主要是mocha, travis这些主流的持续集成和测试框架，当然也含有一些比较基础的数据结构相关的内容，热身的成分还是挺明显的啦。课前内容比较基础，完成的也相对随意。咳咳，感觉是应该要紧张起来进入状态了。

## 预备知识

#### Node Assert

[assert 模块](http://nodejs.cn/api/assert.html)是Node的内置模块，主要用于断言。如果表达式不符合预期，就抛出一个错误。常用的有assert.equal(), assert.fail()等函数。

#### Mocha

简单介绍一下mocha，Mocha（发音"摩卡"）诞生于2011年，是现在最流行的JavaScript测试框架之一，在浏览器和Node环境都可以使用。练习中两个环境都有涉及。

#### TDD

引自wiki
>
测试驱动开发（英语：Test-driven development，缩写为TDD）是一种软件开发过程中的应用方法，由极限编程中倡导，以其倡导先写测试程序，然后编码实现其功能得名。测试驱动开发始于20世纪90年代。测试驱动开发的目的是取得快速反馈并使用“illustrate the main line”方法来构建程序。
>
测试驱动开发是戴两顶帽子思考的开发方式：先戴上实现功能的帽子，在测试的辅助下，快速实现其功能；再戴上重构的帽子，在测试的保护下，通过去除冗余的代码，提高代码质量。测试驱动着整个开发过程：首先，驱动代码的设计和功能的实现；其后，驱动代码的再设计和重构。
>

举一个TDD的例子：我们想要实现一个对象的几个功能，我们可以先只定义接口和它期望的行为比如它的返回值应该是什么啊等等，为它编写测试用例而不是先实现功能，这时我们开始测试，通常情况下GUI会飘红来警示我们测试失败，然后我们为它编写功能代码以通过测试；假如一切顺利，GUI会显示绿色来告诉我们测试通过了（XCode中的XUnit组件有类似的行为），最后再在保证测试通过的情况下重构代码。所以TDD很多时候又被口语化的叫做红绿测试(red green test)

#### BDD

引自wiki
>
>行为驱动开发（英语：Behavior-driven development，缩写BDD）是一种敏捷软件开发的技术，它鼓励软件项目中的开发者、QA和非技术人员或商业参与者之间的协作。BDD最初是由Dan North在2003年命名[1]，它包括验收测试和客户测试驱动等的极限编程的实践，作为对测试驱动开发的回应。在过去数年里，它得到了很大的发展[2]。
>
2009年，在伦敦发表的“敏捷规格，BDD和极限测试交流”[3]中，Dan North对BDD给出了如下定义：
>
BDD是第二代的、由外及内的、基于拉(pull)的、多方利益相关者的(stakeholder)、多种可扩展的、高自动化的敏捷方法。它描述了一个交互循环，可以具有带有良好定义的输出（即工作中交付的结果）：已测试过的软件。
>

我对BDD没有什么切身体会，我的看法是BDD或许更侧重与协调解决需求与开发联系不畅的问题？毕竟也只有需求部分才需要非技术人员参与了。

#### should.js

引自[官网](https://github.com/shouldjs/should.js)
>
should is an expressive, readable, framework-agnostic assertion library. The main goals of this library are to be expressive and to be helpful. It keeps your test code clean, and your error messages helpful.
>

简单来说，should.js能够帮助我们以一种非常英语口语化的方式来编写测试用例，它的api都是由类似于should,have,be,and这样的词构成的。举个例子，it should be就是中文中它应该是...的意思，对于native speaker和学习过英语的人来说非常直观。

#### karma

>
Karma是Testacular的新名字，在2012年google开源了Testacular，2013年Testacular改名为Karma。
Karma是一个基于Node.js的JavaScript测试执行过程管理工具（Test Runner）。该工具可用于测试所有主流Web浏览器，也可集成到CI（Continuous integration）工具，也可和其他代码编辑器一起使用。
>


关于karma的来龙去脉可以看看这篇[文章](http://taobaofed.org/blog/2016/01/08/karma-origin/)。概括的讲，karma和mocha都可以用于node.js的单元测试，但是karma在浏览器端的单元测试更为方便更有优势，相比之下mocha在浏览器端的单元测试还需要开发者编写适配脚本比较麻烦。karma还有高扩展性，支持远程控制等优点。

#### Travis CI

这里引用[阮一峰的日志](http://www.ruanyifeng.com/blog/2017/12/travis_ci_tutorial.html)介绍一下 Travis CI 和 持续集成，

>
Travis CI 提供的是持续集成服务（Continuous Integration，简称 CI）。它绑定 Github 上面的项目，只要有新的代码，就会自动抓取。然后，提供一个运行环境，执行测试，完成构建，还能部署到服务器。
>
持续集成指的是只要代码有变更，就自动运行构建和测试，反馈运行结果。确保符合预期以后，再将新代码"集成"到主干。
>
持续集成的好处在于，每次代码的小幅变更，就能看到运行结果，从而不断累积小的变更，而不是在开发周期结束时，一下子合并一大块代码。
>

总的来说CI可以为我们merge, push等设计到代码改动的操作提供一方面的保护，我们完全可以通过改动后的代码是否通过测试来对这一次的改动质量做出一个初步判断。并不是通过CI测试的代码就是高质量的代码，但显然的是，如果测试都通过不了，这次改动显然是有问题的。

结合CI，类似于iOS打包的工作也变得简便了，通过每一次提交都自动生成新包这样一种方式，节省了公司分包的时间，减轻了iOS工程师在打包方面的工作量。

## exerciese 1

练习一大概的内容是在github就是fork给定的repo，用npm全局安装了mocha之后，先在repo的根目录下用mocha跑一下测试，当然跑完之后肯定是不过的啦，然后修改根目录下的test/test.js文件，使得test.js文件内三个测试用例能够在mocha下通过。

在全局安装的时候遇到一点小坑。在命令行运行下面的代码

```
npm i -g mocha
```
报了以下的错误

```
npm WARN checkPermissions Missing write access to /usr/local/lib/node_modules
npm ERR! path /usr/local/lib/node_modules
npm ERR! code EACCES
npm ERR! errno -13
npm ERR! syscall access
npm ERR! Error: EACCES: permission denied, access '/usr/local/lib/node_modules'
npm ERR!  { Error: EACCES: permission denied, access '/usr/local/lib/node_modules'
npm ERR!   stack: 'Error: EACCES: permission denied, access \'/usr/local/lib/node_modules\'',
npm ERR!   errno: -13,
npm ERR!   code: 'EACCES',
npm ERR!   syscall: 'access',
npm ERR!   path: '/usr/local/lib/node_modules' }
npm ERR!
npm ERR! The operation was rejected by your operating system.
npm ERR! It is likely you do not have the permissions to access this file as the current user
npm ERR!
npm ERR! If you believe this might be a permissions issue, please double-check the
npm ERR! permissions of the file and its containing directories, or try running
npm ERR! the command again as root/Administrator (though this is not recommended).

npm ERR! A complete log of this run can be found in:
npm ERR!     /Users/zhoujialei/.npm/_logs/2018-11-03T18_29_32_947Z-debug.log
```
大概的意思是当前用户的权限有问题，无法安装到npm的安装目录：/user/local/lib/node_modules。显然这个锅得我自己背，必定是猴年马月的时候运行了类似于

```
sudo npm i -g xxx // 改变了/user/local/lib/node_modules的权限
```
的命令，导致当前用户没法写入了。解决的办法在报错信息上写了，用finder右击打开这个文件，更改一下当前用户的读写权限就能解决了。

![截图](img/1-1.png)

#### case 1

```
describe('Array', function() {
  describe('#indexOf()', function() {
    it('should return -1 when the value is not present', function() {
      assert.equal(-1, [1, 2, 3]/* 填空题 */)
    })
  })
})
```
相信大部分前端同学对indexOf()这个函数不陌生，Array.prototype.indexOf()接受两个参数，第一个参数是要在array内搜索的元素，第二个参数是可选的，表示要从那个索引开始找起，返回值或者是第一个和搜索的元素相同的元素的索引，或者是-1表示要搜索的元素不在数组内。

那要满足assert.equal这个条件，显然我们只要保证我们对[1, 2, 3]这个数组调用indexOf函数的时候，不去寻找1, 2, 3就可以了。下面是能够保证测试通过的代码

```
describe('Array', function() {
  describe('#indexOf()', function() {
    it('should return -1 when the value is not present', function() {
      assert.equal(-1, [1, 2, 3].indexOf(4))
    })
  })
})
```

#### case 2

```
describe('assert', function () {
  it('a和b应当深度相等', function () {
    var a = {
      c: {
        e: 1
      }
    }
    var b = {
      c: {
        e: 1
      }
    }
    // 修改下面代码使得满足测试描述
    assert.equal(a, b)
  })

```

object的深度相等问题；要判断两个object的深度是否相等，我们需要先明确object的深度的定义。

object的深度可以递归的定义如下：

1. **没有非继承属性**或者**所有非继承属性不是object**的object其深度为**1**
2. **有类型为object的非继承属性**的object其深度为**所有类型为object的非继承属性它们的深度中的最大值+1**

根据定义写出depth函数的代码就行了，废话不多说，直接贴代码

```
 it('a和b应当深度相等', function () {
    var a = {
      c: {
        e: 1
      }
    }
    var b = {
      c: {
        e: 1
      }
    }

		
    function depth(e) {
      if (typeof e !== 'object') return -1; // 要是不是object就返回-1了
      let maxDepth = 1;

      for (let property in e)	{
      if (!e.hasOwnProperty(property)) continue;
		if (typeof e[property] === 'object') {
		  let currentDepth = 1 + depth(e[property]);
		  maxDepth = Math.max(maxDepth, currentDepth);
		}
      }

      return maxDepth;
    }
   
    assert.equal(depth(a), depth(b))
  })

```

#### case 3

```
  it('可以捕获并验证函数fn的错误', function () {
    function fn() {
      xxx;
    }
    // 修改下面代码使得满足测试描述
    fn()
  })

```

捕获并验证函数fn的错误；主要考察[assert.throws()函数](http://nodejs.cn/api/assert.html#assert_assert_throws_fn_error_message)的使用，当然我们需要先跑一下fn()看看它报什么错然后对症下药利用assert.throws()构建一个测试用例就行了，直接上代码。

```
  it('可以捕获并验证函数fn的错误', function () {
    function fn() {
      xxx;
    }

    // 修改下面代码使得满足测试描述
    var err = new ReferenceError('xxx is not defined');

    // node version >= 9.9.0 之后也可以写成下面这样
    // assert.throws(fn, {
    //   name: 'ReferenceError',   
    //   message: 'xxx is not defined',
    // })

    assert.throws(fn, err)
  })

```
期间遇到一个小坑，在用mocha跑case 3的时候反复报错

```
 1) assert
       可以捕获并验证函数fn的错误:
	TypeError: expected.test is not a function
```

在公司的机子上我的代码没有问题，但是在自用的mac上这个错误始终无法消除。最后对比一下node的版本差异发现是自用的mac上node版本过旧导致的，升级之后case自然就过了。

## exercise 2

练习二的内容形式上跟练习一相近，fork repo然后改test/test.js文件，最后通过mocha跑通测试用例就行了，除此之外还有接入travis ci的步骤要求。

不像练习一考察了assert API的使用，这里主要考察的是数据结构的内容，上代码。

```
// test/test.js
var add = require('../lib/add')

describe('大数相加add方法', function () {
  it('字符串"42329"加上字符串"21532"等于"63861"', function () {
    add('42329', '21532')
      .should.equal('63861')
  })
  
  it('"843529812342341234"加上"236124361425345435"等于"1079654173767686669"', function () {
    add('843529812342341234', '236124361425345435')
      .should.equal('1079654173767686669')
  })
})

// lib/add.js
function add() {
  // 实现该函数
}

module.exports = add

```

实现一个add函数保证以字符串形式表示的数字相加运算成立，结果也以字符串形式返回。

事实上对于多位数与多位数相加，我们可以利用竖式加法的思路来处理：记个位为第一位，十位为第二位，以此类推；两个数第n位(n >= 1)相加的结果为**两个数第n位相加的值加上进位的值余10**，其中**进位的值为1当且仅当两个数第n-1位相加的值大于10**，根据这样的思路将两个数各个数位对应相加就能得到结果。这里可以考虑使用array作为存储结果的数据结构，array的index从小到大依次存储sum的第一位，第二位等等，这样做的好处是我们不需要考虑sum究竟是几位的问题，充分利用了javaScript的数组长度可变的特性。下面是实现代码

```
function add(lhs, rhs) {
  let resultArray = []; // store sum, in reverse order
  let lhsCursor = lhs.length-1;
  let rhsCursor = rhs.length-1;
  let arrayCursor = 0;
  let optIn = 0; // 进位
  while (lhsCursor >= 0 && rhsCursor >= 0) { // 被加数和加数的位数还未穷尽
    let result = Number(lhs[lhsCursor]) + Number(rhs[rhsCursor]);
    resultArray[arrayCursor] = result % 10 + optIn;
    optIn = parseInt(result / 10);
    lhsCursor--;
    rhsCursor--;
    arrayCursor++;
  }
  while (lhsCursor >= 0) { // 加数的位数已经穷尽
    let result = Number(lhs[lhsCursor]) + optIn;
    resultArray[arrayCursor] = result % 10 + optIn;
    optIn = parseInt(result / 10);
    lhsCursor--;
    arrayCursor++;
  }
  while (rhsCursor >= 0) { // 被加数的位数已经穷尽
    let result = Number(rhs[rhsCursor]) + optIn;
    resultArray[arrayCursor] = result % 10 + optIn;
    optIn = parseInt(result / 10);
    rhsCursor--;
    arrayCursor++;
  }
  if (optIn === 1) { // 最高位相加的和大于等于10
    resultArray[arrayCursor] = 1;
  }

  return resultArray.reverse().join(''); // 存放sum的数组是从小位往大位存放的，显示的结果需要与之相反
}

```

接入Travis CI的步骤就比较机械了，跟着[travis 官方网站 tutorial](https://docs.travis-ci.com/user/tutorial/)把步骤走下来就好了，大概的步骤如下：

* 在travis-ci.com上登陆你的github账号并且同意 Travis CI 认证
* 点击激活按钮并选择你想要接入 Travis CI 的repo
* 把.travis.yml文件放到你的repo中，这个文件用来提供 Travis CI 启动时需要的信息，我们是在node js下使用mocha测试，可以这样写：

```
language: node_js
node_js: // 指定使用8, 7, 6这三个版本的node js测试测试用例，可以忽略
  - "8"
  - "7"
  - "6"

```

直接接入练习二会报找不到mocha的错，我们是在本地环境下全局安装mocha的，Travis CI提供的容器里并不是天然带上mocha的，我的做法是在练习二项目里的package.json加上mocha的devDependencies

```
// package.json
  ...
  "devDependencies": {
    "mocha": "^5.2.0", // add this line to your devDependencies
    "should": "^11.2.1"
  }
  ...
```

这样 Travis CI 在初始化我们的repo时就会带上mocha了，至于测试通过那当然也是顺理成章的事情。

![测试通过](img/course/week-1/2-1.png)

## exercise 3

练习三主要考察的是karma的使用以及它和 Travis ci 的配合使用。和前面的练习一样，要求我们修改test/test.js文件来通过测试用例，并能够在 Travis CI 的环境下通过测试。

下列是karma的初始化配置
>
1. Which testing framework do you want to use ? (mocha)
2. Do you want to use Require.js ? (no)
3. Do you want to capture any browsers automatically ? (Chrome)
4. What is the location of your source and test files ? (https://cdn.bootcss.com/jquery/2.2.4/jquery.js, node_modules/should/should.js, test/**.js)
5. Should any of the files included by the previous patterns be excluded ? ()
6. Do you want Karma to watch all the files and run the tests on change ? (yes)
>

配置完成后在根目录下会生成karma.conf.js文件，这是karma的配置文件，之后我们还会跟它打交道。

```
// test/test.js
describe('jQuery', function () {
  it('should have jQuery', function () {
    if (!window.jQuery) {
      throw new Error('查看下 karma.conf.js 配置项 files 是否正确')
    }
  })

  it('should able to get a body', function () {
    var $body = $('body')
    $body.length.should.equal(1)
    $body[0].should.equal(document.getElementsByTagName('body')[0])
  })

  describe('should able to trigger an event', function () {
    var ele
    before(function () {
      ele = document.createElement('button')
      document.body.appendChild(ele)
    })

    it('should able trigger an event', function (done) {
      $(ele).on('click', function () {
        done()
      }).trigger('click')
    })

    after(function () {
      document.body.removeChild(ele)
      ele = null
    })
  })

  it('should able to request https://raw.githubusercontent.com/FE-star/exercise1/master/test/test.js', function (done) {
    // 使用 jQuery.ajax 请求 https://raw.githubusercontent.com/FE-star/exercise1/master/test/test.js，并验证是否拿到文件
  })
})
```

使用$.ajax()请求对应的url，在请求成功的回调里面调用done()函数即可满足需求，关于done()函数部分的细节见[官网](https://mochajs.org/)的 **ASYNCHRONOUS CODE** 部分示例代码即可。

我选择chrome作为karma的浏览器端测试环境，.travis.yml的具体配置见下文

```
language: node_js
addons:
  chrome: stable
node_js:
  - "8"
  - "7"
  - "6"
```

#### 遇到的几个小问题

在练习三接入 Travis CI 后发现job log输出下列语句并且job处于挂起状态

```
02 11 2018 09:00:48.600:ERROR [launcher]: Cannot start Chrome

```

查阅[官方文档](https://docs.travis-ci.com/user/chrome#sandboxing)发现需要在karma.conf.js里面进一步配置customLaunchers相关选项。根据官网的说明配置发现错误还是没有解决，最后在[karma-chrome-launcher issue](https://github.com/karma-runner/karma-chrome-launcher/issues/158)中终于找到了正确的配置代码

```
// karma.conf.js
...
browsers: ['ChromeHeadlessNoSandbox'],
customLaunchers: {
  ChromeHeadlessNoSandbox: {
    base: 'ChromeHeadless',
    flags: ['--no-sandbox']
  }
},
...
```

这样测试用例终于跑起来了，但是又遇到新的问题:

```
// job log on Travis CI

02 11 2018 09:20:34.891:INFO [karma]: Karma v1.7.1 server started at http://0.0.0.0:9876/
02 11 2018 09:20:34.892:INFO [launcher]: Launching browser ChromeHeadlessNoSandbox with unlimited concurrency
02 11 2018 09:20:34.896:INFO [launcher]: Starting browser ChromeHeadless
02 11 2018 09:20:35.804:INFO [HeadlessChrome 70.0.3538 (Linux 0.0.0)]: Connected on socket dsU78mwC-v8B-cR3AAAA with id 50944414
HeadlessChrome 70.0.3538 (Linux 0.0.0): Executed 0 of 4 SUCCESS (0 secs / 0 secs)
e 70.0.3538 (Linux 0.0.0): Executed 1 of 4 SUCCESS (0 secs / 0 secs)
e 70.0.3538 (Linux 0.0.0): Executed 2 of 4 SUCCESS (0 secs / 0.003 secs)
e 70.0.3538 (Linux 0.0.0): Executed 3 of 4 SUCCESS (0 secs / 0.067 secs)
e 70.0.3538 (Linux 0.0.0): Executed 4 of 4 SUCCESS (0 secs / 0.068 secs)
e 70.0.3538 (Linux 0.0.0): Executed 4 of 4 SUCCESS (0.077 secs / 0.068 secs)
No output has been received in the last 10m0s, this potentially indicates a stalled build or something wrong with the build itself.
Check the details on how to adjust your build configuration on: https://docs.travis-ci.com/user/common-build-problems/#Build-times-out-because-no-output-was-received
The build has been terminated
```

如果测试用例本身没有问题的话，那就只能是我们的karma配置需要进一步改进。翻阅karma.conf.js发现**配置项singleRun**值为false，查阅[官方文档](http://karma-runner.github.io/3.0/config/configuration-file.html)并猜想测试timeout是不是由于测试完成之后没有退出导致的。由于我们的测试确实跑一次也就够了，把**配置项singleRun**的值改为true。Hooray！问题解决，Travis CI 终于pass了我们的测试。下面是正确配置后的 Travis CI job log:

```
02 11 2018 09:49:36.167:INFO [karma]: Karma v1.7.1 server started at http://0.0.0.0:9876/
02 11 2018 09:49:36.170:INFO [launcher]: Launching browser ChromeHeadlessNoSandbox with unlimited concurrency
02 11 2018 09:49:36.175:INFO [launcher]: Starting browser ChromeHeadless
02 11 2018 09:49:37.037:INFO [HeadlessChrome 70.0.3538 (Linux 0.0.0)]: Connected on socket XB4iFLV-T3LSRX0jAAAA with id 81911669
HeadlessChrome 70.0.3538 (Linux 0.0.0): Executed 0 of 4 SUCCESS (0 secs / 0 secs)
e 70.0.3538 (Linux 0.0.0): Executed 1 of 4 SUCCESS (0 secs / 0 secs)
e 70.0.3538 (Linux 0.0.0): Executed 2 of 4 SUCCESS (0 secs / 0.003 secs)
e 70.0.3538 (Linux 0.0.0): Executed 3 of 4 SUCCESS (0 secs / 0.121 secs)
e 70.0.3538 (Linux 0.0.0): Executed 4 of 4 SUCCESS (0 secs / 0.123 secs)
e 70.0.3538 (Linux 0.0.0): Executed 4 of 4 SUCCESS (0.132 secs / 0.123 secs)
The command "npm test" exited with 0.
Done. Your build exited with 0.
```
