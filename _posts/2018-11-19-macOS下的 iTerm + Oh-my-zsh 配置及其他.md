---
layout:     post
title:      "macOS下的 iTerm + Oh-my-zsh 配置及其他"
subtitle:   "借助oh-my-zhs的theme和plugin，命令行也可以很酷炫"
date:       2018-11-18
author:     "zhoujialei"
header-img: "img/post-bg-unix-linux.jpg"
tags:
    - 开发
    - front-end
    - cmd
---

最近在重度使用`vim`，强迫自己去学习这个历史悠久但是学习曲线也特别陡峭的编辑器。熟悉了之后发现vim比我想象中的要灵活的多，同时也“笨重”的多，这里的笨重指的是：一个再熟悉`vim`的人，如果不借助社区贡献的种种`plugin`，也很难发挥`vim`的真正威力。而且阻碍`vim`发挥其威力的不只是掌握`vim`本身，如果不能对命令行操作融会贯通，使用`vim`的效果也会大打折扣。这就是本篇文章的目的了，一个productive的`shell`加上一个productive的`terminal`，在这些工具的帮助下，专注于`vim`本身的学习与使用就不再是问题了。

## zsh

`macOS`下默认使用`bash`，知乎上有个[问题](https://www.zhihu.com/question/21418449)比较详细的说明了为什么我们最好使用`zsh`替换`bash`。从我的日常使用出发，如果没有`zsh`的目录自动补全，目录的选择模式，而是让我简简单单使用`ls`来`print`当前目录文件，使用`cd`进入想要的目录，我是万万不干的: 如此没有效率的`workflow`，我不如在`finder`上用鼠标操作进行目录切换了。除此之外，`zsh`还可以搭配其他`plugin`发挥更大作用(`bash`或许有，但是恕我孤陋寡闻没有听过): 安装`z plugin`实现目录自动跳转啦，通过`jump plugin`为常用的目录增加“书签”啦，通过`osx plugn`在命令行操作一些`macos`特有的`feature`啦等等，而学习和使用这些插件可以说几乎是没有成本的。

### install

使用`homebrew`安装`zsh`应该是一件比较干净利落的事吧:

```
brew install zsh
```

## iTerm

呐，关于`iTerm`为什么优于`macOS`自带的`terminal.app`的细节这里就不多赘述了，知乎上也有大佬对[这个问题](https://www.zhihu.com/question/27447370)有详细的阐述。我在这里只说说我常用到的命令: `command+D`分屏，有时候在`vim`编辑的时候单buffer不能满足我的要求，比如在`electron + react`本地调试的时候，需要既本地运行`electron`又运行`react`，可能还需要在`vim`内对代码做更改，这个时候我完全可以在不退出`vim`的时候通过`command+D`另起炉灶，定位到当前`project`，执行这些操作，并在完成这些操作后关掉它。这最大程度避免了对我编辑代码时思路的干扰。至于其他的`features`比如选中即复制啊，`command+shift+h`打开`paste history`啊等等，相信读者结合知乎大佬的回答和自己的使用，应该就会有更深的体会。

### install

[官网](https://www.iterm2.com/)里面大大的Download按钮，点击它就可以开始下载了！下载完成就得到了`DMG`文件，然后....


## Oh-my-zsh

> Oh My Zsh is an open source, community-driven framework for managing your zsh configuration.

总的来说，`oh-my-zsh`是一个管理`zsh`配置的框架。听起来感觉平平无奇，官网还有关于它的另外一种描述:

> Oh My Zsh will not make you a 10x developer...but you might feel like one.

嗯，如果你用了它，它不会真的把你变🐂，但是你可能会感觉自己变🐂。然后还有:

> Once installed, your terminal shell will become the talk of the town or your money back! With each keystroke in your command prompt, you'll take advantage of the hundreds of powerful plugins and beautiful themes. Strangers will come up to you in cafés and ask you, "that is amazing! are you some sort of genius?"

对，重点在于来自活跃社区的丰富的插件和主题支持，这才是`oh-my-zsh`的优势所在。结合这些插件和主题，你的`shell`便能够变得功能更加丰富，界面更加美观。

勤奋的同学可能已经把[官网上的说明](https://github.com/robbyrussell/oh-my-zsh)过了一遍了，而懒惰的同学，特别是懒惰的前端开发同学，只需要继续往下看就好了。至少出于我个人的经验，接下来要介绍的`oh-my-zsh`的`theme`配置和一些插件的使用，应该就能`cover`掉大部分情况下开发的`shell`使用了。

### install

官网介绍了通过`curl`和通过`wget`两种方式:

`sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)" # curl`
`sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)" # wget`

### plugins的配置

安装完成之后，你可以在[这个wiki](https://github.com/robbyrussell/oh-my-zsh/wiki/Plugins)上浏览`plugin`并选择你想要的`plugin`并安装，之后在`~/.zshrc`里面更改`plugins=(...)`括号内的内容就算添加成功了，当然要使配置生效，请确保`plugin`已经安装了，并且要重新启动`shell`或者执行`source ~/.zshrc`就可以了。

### theme的配置

未经更改的情况下，在`~/.zshrc`中有这么一行代码:

```
ZSH_THEME="robbyrussell"
```
这行代码决定了`oh-my-zsh`的`theme`配置，关于配置的可选项，[wiki](https://github.com/robbyrussell/oh-my-zsh/wiki/themes)上也有相应的介绍并配有截图，当然部分`theme`可能还要求用户安装
[Powerline Fonts](https://github.com/powerline/fonts)保证字体的正常渲染，安装流程见链接就行了。更改完`theme`之后重新启动`shell`就可以看到立竿见影的效果了。

### 几个我常用的plugin

#### z

`oh-my-zsh`自带`z plugin`，只需要在`~/.zshrc`中确认`plugins=(...)`括号中是不是有`z`的存在，没有补上就可以使`z`生效了。应用的场景也很普遍: 总有些目录是我们经常`cd`过去的，但是这些目录总有一些目录深度很深，利用`jump`来为他们一一打上标签也总是显得没那么必要，这个时候`z`就能派上大用场了。在这些目录是高频率使用的情况下，只需要在`iTerm`中输入`z`加上想要去的目录的关键词，然后按下`tab键`，duang！目录就自动被补全了，再敲击回车便能到达我们想要去的目录。在有多个目录相似的情况或者没有z的结果没有匹配到我们想要的目录时，多次敲击tab键进入选择模式，在大部分情况下也能选到我们想去的目录从而解决问题。

#### jump

`z plugin`虽然好用但是并不是非常可靠，特别是当高频使用目录很相似的情况下，`z plugin`自动补全的路径往往并不是我们想要去的路径。如果想要更彻底的控制这个行为，`jump plugin`就是一个很好的选择。

它的使用也很简单: 我们可以为我们常去的目录定义一个“书签名“，比如我们想把`~/Desktop/tempDirectory`定义为`temp`，只需要`cd`到`~/Desktop/tempDirectory`之下，然后执行`mark temp`,这样我们就为这个路径添加了一个叫做`temp`的`书签`了。当我们想要`cd`到这个路径的时候，只需要执行`jump temp`就能够达到目的了。在命令行中输入`marks`可以看到所有当前定义的书签，避免遗忘。

#### osx

[官网](https://github.com/robbyrussell/oh-my-zsh/tree/master/plugins/osx)有对`osx`的详细介绍，在这三个插件里面，这个算是比较不实用的了。我们可能需要在`Finder`中打开当前目录，输入`open .`能够达到目的。在`osx plugin`的帮助下，输入`ofd`就可以了，其他的用法在链接中就可以找到，各位见仁见智吧。



