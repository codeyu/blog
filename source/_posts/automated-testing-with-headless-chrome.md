---
title: 用 Headless Chrome 进行测试自动化
date: 2017-06-18 21:42:22
categories: 翻译
tags: [chrome, node.js, test]
copyright: false
description:
---
<!--more-->
如果你想用 Chrome 浏览器的 Headless 模式运行自动化测试，不用东奔西走。这篇文章就是教你怎么使用 Karma 和 Mocha+Chai 进行测试。

**你说的啥？**（黑人问号）

Karma, Mocha, Chai, Headless Chrome,  你居然不知道？？？

[Karma](https://karma-runner.github.io/) 是一个测试套件，它适用于任何最受欢迎的测试框架(如 [Jasmine](https://jasmine.github.io/), [Mocha](https://mochajs.org/), [QUnit](https://qunitjs.com/) 等).

[Chai](http://chaijs.com/) 是工作在 Node 和浏览器中的断言库，我们在浏览器中使用它。

[Headless Chrome](https://developers.google.com/web/updates/2017/04/headless-chrome) 是运行 chrome 浏览器的一种方式，它在没有用户界面的无头环境中运行。用 Headless Chrome 的好处之一是你的 JavaScript 测试代码的运行环境和你网站的用户是一样的(而不是直接在 Node 中测试) 。Headless Chrome 给你一个真正的浏览器环境但没有完整版 chrome 浏览器那样的内存开销。

## 设置

### 安装
用 yarn 安装 Karma，相关的插件，和测试工具：
```
yarn add --dev karma karma-chrome-launcher karma-mocha karma-chai
yarn add --dev mocha chai
```
或者使用 npm:
```
npm i --save-dev karma karma-chrome-launcher karma-mocha karma-chai
npm i --save-dev mocha chai
```
在这篇文章中我使用 Mocha  和 Chai，但如果你不想用，你可以选择任何你喜欢的可以在浏览器中使用的断言库。

### 配置 Karma

为了使用 ChromeHeadless 启动器，需创建 `karma.config.js` 文件。

**karma.conf.js**
```
module.exports = function(config) {
  config.set({
    frameworks: ['mocha', 'chai'],
    files: ['test/**/*.js'],
    reporters: ['progress'],
    port: 9876,  // karma web server port
    colors: true,
    logLevel: config.LOG_INFO,
    browsers: ['ChromeHeadless'],
    autoWatch: false,
    // singleRun: false, // Karma captures browsers, runs the tests and exits
    concurrency: Infinity
  })
}
```

<aside class="note">

注意： 执行 ` ./node_modules/karma/bin/ init karma.conf.js` 去生成 Karma 配置文件。

</aside>

## 写一个测试
在 ` /test/test.js`  中写测试代码。

**/test/test.js**
```
describe('Array', () => {
  describe('#indexOf()', () => {
    it('should return -1 when the value is not present', () => {
      assert.equal(-1, [1,2,3].indexOf(4));
    });
  });
});
```
## 执行你的测试

为了根据我们的设置执行 Karma，需要在 `package.json` 文件中添加测试脚本命令：

**package.json**
```
"scripts": {
  "test": "karma start --single-run --browsers ChromeHeadless karma.conf.js"
}
```
当你执行测试命令（`npm test` 或 `yarn test`），Headless Chrome
将被激活运行并且输出结果到终端：
![](https://github.com/codeyu/headless-karma-travis/blob/master/mac%20headless%20chrome%20test.png?raw=true)

<aside class="caution">

**警告：** 

**Chrome 59** 的无头模式当前在 Mac 和 Linux 系统可用。

[Windows 支持](https://bugs.chromium.org/p/chromium/issues/detail?id=686608) 要等到 **Chrome 60**。 

查看你的浏览器的版本，在地址栏输入： `chrome://version` 。

</aside>

## 创建你自己的 Headless Chrome 启动器

使用 `ChromeHeadless` 启动器在 Headless Chrome 上开箱即用，它包括合适的 Chrome 参数和固定的远程调试端口 `9222`。这挺不错的。

尽管如此，有时候你可能想要给 Chrome 传递个性化的参数或者改变启动器使用的远程调试端口。有鉴于此，可以扩展 `ChromeHeadless` 启动器创建 `customLaunchers` 配置项：

**karma.conf.js**
```
module.exports = function(config) {
  ...

  config.set({
    browsers: ['Chrome', 'ChromeHeadless', 'MyHeadlessChrome'],

    customLaunchers: {
      MyHeadlessChrome: {
        base: 'ChromeHeadless',
        flags: ['--disable-translate', '--disable-extensions', '--remote-debugging-port=9223']
      }
    },
  }
};
```
## 使用 Travis CI 持续集成
 在 Headless Chrome 配置 Karma 执行测试任务并不轻松。
不过使用 Travis 持续集成只需要几行代码！
为了在 Travis 中 执行测试命令，需要使用 `dist: trusty` 并且安装 稳定版 Chrome 插件：

**.travis.yml**
```
language: node_js
node_js:
  - "7"
dist: trusty # needs Ubuntu Trusty
sudo: false  # no need for virtualization.
addons:
  chrome: stable # have Travis install chrome stable.
cache:
  yarn: true
  directories:
    - node_modules
install:
  - yarn
script:
  - yarn test
```
<aside class="note">

注意：完整例子程序在 [headless karma travis](https://github.com/codeyu/headless-karma-travis)

</aside>

> 原文地址： [Automated testing with Headless Chrome](https://developers.google.com/web/updates/2017/06/headless-karma-mocha-chai)