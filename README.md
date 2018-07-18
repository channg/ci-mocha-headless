## 使用CI、headless Browser、mocha对前端代码进行测试

### 什么是CI

>CI 提供的是持续集成服务（Continuous Integration，简称 CI）。持续集成指的是只要代码有变更，就自动运行构建和测试，反馈运行结果。

### 什么是headless Browser
> headless Browser 中文翻译，无头浏览器。是一种没有界面的浏览器，可以在命令窗口中被运行。

### 什么是mocha
> ☕️ Simple, flexible, fun JavaScript test framework for Node.js & The Browser ☕️
> 是一种可以运行在浏览器以及nodejs 环境的前端测试框架

### 为什么要编写测试代码
对于迭代需求，我们人类编写的代码，只能保证在当前事件节点的正确性，随着事件的推移，代码的变动，以及人为关系。我们无法保证之前的逻辑完全符合曾经的要求，这时候我们就需要编写测试代码对功能点进行测试。
测试不是一次性的，而是`持续的`，`永久的`。

对于开源框架而言，测试的覆盖面积更代表了框架的可靠性；也能使用自动化测试更好的约束贡献者提交的PR

### 开始使用mocha 对代码进行测试

首先呢，我要开始编写一个`add.js`含有一个方法`add`，这个方法我希望获取 `a+b` 的值是一个`Number`
```javascript
function return(a,b){
	reutnr a+b
}
	
```

好的现在我可以对这个方法进行测试了

增加一个用于测试`index.html`

```vbscript-html
<!DOCTYPE html>
<html>
<head>
    <title>Mocha Test</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <!-- 引用mocha css -->
    <link rel="stylesheet" href="https://cdn.bootcss.com/mocha/5.2.0/mocha.css" />
</head>
<body>
<div id="mocha"></div>
<!-- 引用chai.js chai.js是一个断言工具库 -->
<script src="http://www.chaijs.com/chai.js"></script>
<!-- 引用mocha.js -->
<script src="https://cdn.bootcss.com/mocha/5.2.0/mocha.js"></script>
<!-- 引用我们的功能代码add.js -->
<script src="../add.js"></script>
<!-- 开启bdd  这段代码可以看这里 https://github.com/mochajs/mocha/issues/2719 -->
<script>mocha.setup('bdd');</script>
<!-- 引用我们的测试代码test.js -->
<script src="test.js"></script>
<script>
  mocha.run();
</script>
</body>
</html>
```

编写  `test.js`
>chai.expect是一个chai.js的断言方法，如果出现问题，则会抛出一个异常 文档地址 http://www.chaijs.com/

>describe 是mocha的测试取款，每一个it都会执行一个测试用例
```javascript
var expect = chai.expect;

describe('ADD.JS TEST', function() {
  it('add(1,1)', function() {
    expect(add(1,1)).to.equal(2)
  });
})
```

现在我们就可以直接运行`index.html`查看效果了，当然可以开启一个http服务器查看，可以使用`http-server`快速开启一个`http`服务。

在浏览器运行会出现以下提示，表示测试通过

<img src="https://i.h2.pdim.gs/a428d4844ba06700ca7cf614e342ff3d.png">

如果想要添加更多测试用例可以继续添加更多的测试代码。

### 接下来

很显然，我们在持续编写`add.js`的时候，并不愿意每次都打开网页去运行并查看代码测试情况。

这时候，`headless`要开始大展拳脚了。

使用`mocha-chrome` 直接在命令行运行`mocha`测试用例。`mocha-chrome`是一个可以在命令行对`mocha`页面进行测试的框架。它可以将测试结果展现在控制台。

```
npm init 
...
npm i mocha-chrome --save-dev
```
修改`package.json`增加
```
  "scripts":{
    "test": "mocha-chrome ./test-some/index.html"
  }
```
调用命令
```
npm test
```
这时候，正确的提示会出现在控制台中
<img src="https://i.h2.pdim.gs/1aa6afee8284df9bbde40f3280c960bf.png"/>

项目地址https://github.com/channg/ci-mocha-headless

这时候，当我们测试项目的时候就不必须打开浏览器去检查代码是否通过验证了，只要输入`npm test`就可以在控制台看到效果，是不是变得很轻松了呢。

### 继续

当我们测试用例过长，占用时间过多，或者需要其他前置操作，或者需要测试多个版本，多个系统的兼容性。我们应该如何做呢。很明显，要使用`CI`。

所以，我们并不想每次在本地进行测试，这里我们将要使用`travis ci`。

关于`travos ci`我们可以阅读<a href="http://www.ruanyifeng.com/blog/2017/12/travis_ci_tutorial.html">阮一峰的博文</a>

在项目根目录创建`.travis.yml`
内容如下。
```
sudo: required
language: node_js
node_js:
  - "8"
  - "9"
```

当你在`travos ci`开启了`repository you want to build`按钮的时候。每次项目的提交就会触发`ci`的构建。

而且你可以生成github小图标放在项目的`readme`中，比如说这个[![Build Status](https://travis-ci.org/channg/ci-mocha-headless.svg?branch=master)](https://travis-ci.org/channg/ci-mocha-headless) 是不是很酷。

点击这个小徽章，你就可以查看我的项目在`ci`构建的过程。

查看travis ci 的文档，去获取更多的资料 https://docs.travis-ci.com/

### 结束

基本的测试方法你已经掌握了，现在可以去了解更多了，如果有问题，可以查看我的项目进行对比
https://github.com/channg/ci-mocha-headless

**更多资源**

* https://docs.travis-ci.com/
* http://www.chaijs.com/
* https://mochajs.org/
* https://github.com/shellscape/mocha-chrome