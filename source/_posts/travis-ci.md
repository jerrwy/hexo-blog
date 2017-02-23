---
title: 利用travis-ci对github项目进行自动化单元测试
date: 2017-02-22 22:38:09
tags: CI
---

有没有羡慕github项目上有名的网站readme.md中都有下面这个标志？

[![Build Status](https://travis-ci.org/moyunchen/lifegame.svg?branch=master)](https://travis-ci.org/moyunchen/lifegame)

这个表示你的代码测过了单元测试，有了一定的可靠性。

下面我将一步一步的告诉你怎么给你的github项目添加这个标志。

就比如我这个项目readme，有好多标签，代码测试通过、代码覆盖率、npm下载量等等。[查看](https://github.com/moyunchen/lifegame)

## 单元测试

单元测试（unit testing），是指对软件中的最小可测试单元进行检查和验证。

一般单元测试是针对函数而言，对一个函数进行多组单元测试，用以保证这个函数的可靠性。

---

## node单元测试

在node.js中，我们使用 [mocha](https://github.com/mochajs/mocha) 和 [expect](https://github.com/mjackson/expect)

mocha是一个单元测试框架，expect是一个断言库。(你也可以使用其他的断言库，比如[should](https://github.com/shouldjs/should.js)等)

示例代码

//add.js
``` javascript
function add(a,b){
    return a+b
}

module.exports = add
```

//add.spec.js
``` javascript
const expect = require('expect')
const add = require('./add')

describe('add function', () =>{
    it('should return 2 when 1 + 1', (next) =>{
        expect(add(1,1)).toEqual(2)
        next()
    })
}) 
```

在命令行输入：
``` shell
npm install expect
npm install mocha
mocha add.spec.js
```

结果：
![ut-result](http://oiije1eg7.bkt.clouddn.com/ut-result.png)

即可看到单元测试的结果。

当有多个测试文件时，可以使用 `mocha *` 命令来执行全部文件 

我们也可以使用npm命令跑单元测试。

在package.json文件中scripts中添加：

``` javascript
"scripts": {
    "test": "node ./node_modules/mocha/bin/_mocha --timeout 10000 --bail ./*.spec.js",
  }, 
```

然后运行：
``` shell
npm run test 
```
即可看到单元测试跑起来了。

---

## CI

CI (持续集成)是一种软件开发实践，即团队开发成员经常集成它们的工作，通过每个成员每天至少集成一次，也就意味着每天可能会发生多次集成。

每次集成都通过自动化的构建（包括编译，发布，自动化测试）来验证，从而尽早地发现集成错误。

---

## travis-ci 

[travis-ci](https://travis-ci.org/) 为github开源项目提供了自动化构建的功能。

我们需要做的只需要如下几步：

1. 去 [travis-ci官网](https://travis-ci.org/) 注册账号，用github账号登录。
2. 将我们上面写的代码上传到你的github上。并在travis-ci上选择同步该项目。
3. 在你的项目的根目录添加 `.travis.yml` 文件。
4. 然后在你每次提交代码的时候，travis-ci 都会自动跑单元测试。

如图：
![travis-ci](http://oiije1eg7.bkt.clouddn.com/travis-ci-01.png)

**.travis.yml文件编写**

如下：
``` shell
language: node_js
node_js:
    - "6"
git:
  submodules: false
install:
    - npm install
test:
    - npm run test
```

详细的可以查看 [官方文档](https://docs.travis-ci.com/)

---

## buliding/pass图标

当你的单元测试在travis-ci上面跑通过了，你就有属于你的项目的标签了。

https://travis-ci.org/[github name]/[repository name]

将上面的账号和项目改成你的就行了~

当你的项目正在跑单元测试时，他会显示running.

当你的项目单元测试挂了,他会显示failed.

更多的标签可以[查看这里](http://shields.io/)


