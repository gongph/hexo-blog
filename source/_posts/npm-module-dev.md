---
title: 如何发布自己的模块到NPM社区
date: 2016-06-24 14:01:31
tags:
- npm
- 前端技术
categories:
- npm
---

最基本的npm模块包括两个文件： package.json 和 可执行文件。 可执行文件一般来说是一个入口文件。满足这两点基本上就可以发布自己的模块了。

那么，我们如何发布自己的模块到npm社区呢？
<!-- more -->

#### 创建模块目录
```js
    > mkdir sayHi
    > cd sayHi
```

#### 创建入口文件
```js
    > type null > index.js
```

#### 编辑入口文件
index.js 代码很简单：
```js
    module.exports = function (name) {
        alert('hi, ' + name);
    }
```

#### 创建package.json文件 
在文件根目录执行下面命令：
```js
    > sayHi > npm init
```

package.json文件的内容如下：
```json
{
  "name": "sayhi",
  "version": "1.0.0",
  "description": "say hi to you",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "gongph",
  "license": "ISC"
}
```

**现在的目录结构是：**
```js
    sayHi
      |—— index.js 
      |—— package.json
```

#### 注册npm帐号

完成了上面的步骤之后，我们接下来要在www.npmjs.org注册一个账号，这个账号会被添加到npm本地的配置中，用来发布module用。

```js
    > npm adduser
    Username: your name 
    Password: your password
    Email: your email
```

成功之后，npm会把认证信息存储在~/.npmrc中，并且可以通过以下命令查看npm当前使用的用户:

```js
    > npm whoami
```

#### 发布模块

以上完成之后，我们就可以发布自己的module了，在项目根目录执行以下命令发布模块：

```
    > sayHi > npm publish
```

这个时候，报了一个错，错误信息如下：
{% codeblock lang:java %}
npm ERR publish 403
You do not have permission to publish 'sayhi'.Are you logged in as
the corrent user?:sayhi
{% endcodeblock %}

意思是我没权限发布sayhi，并问我是否使用了正确的账号，那也许是sayhi被别人发布过了吧，所以我修改了package.json文件把name改成了gongph-sayhi。 然后再从新发布一次就ok了。
```js
    + gongph-sayhi@1.0.0
```

至此，我们的模块就已经发布到 [npmjs.org](https://www.npmjs.com/) 上了，登上去看看吧，用户名和密码你上面执行 npm adduser 时已经注册过了。

#### 安装你的模块

模块发布后，我们安装一下自己的模块，新建测试项目，执行一下命令安装：
```js
    > testSayHi > npm i gongph-sayhi -D
```

测试项目的目录结构如下：
```js
    testSayHi
    |—— node_modules
            |—— gongph-sayhi
                    |—— index.js 
                    |—— package.json
    |—— package.json
```


enjoy it ~ 

我是一枚喜欢玩前端的后端攻城狮，技术有限，代码如有错误之处，还请大神劈正！