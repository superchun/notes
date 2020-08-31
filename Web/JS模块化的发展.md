# JS 模块化的发展

## JS 模块化之前的问题

JS 在早期的时候，大多用来执行独立脚本，在 web 提供一定的交互，所以不需要太多的代码。在那个时候，使用 window 对象存储所有变量是非常方便的。随着 JS 和 web 的发展，这样的处理方式会暴露出许多问题，其中最主要的两个就是：**命名冲突**和**对于大型项目的支持**。

### 命名冲突

假设我们有多个 JS 文件，由不同的人员开发和维护，其中两个 JS 文件中有一个相同的方法名或变量名。如果它们两个同时被另一个文件引用了，冲突就会发生。只能其中一个 JS 文件进行改名，代码才能正常运行，这给开发带来了很多的不便。下面是一个命名冲突的例子：

```js
// greeting.js
var helloInLang = {
  en: 'hello world',
  es: '¡Hola mundo!'
}
function writeHello(lang) {
  document.write(helloInLang[lang])
}

// hello.js
function writeHello() {
  document.write('The script is broken')
}
```

### 对于大型项目的支持

当使用 JS 构建一个大型项目时，在大多数 ES5 浏览器环境中，需要显式地使用`script`标签插入 JS 脚本。如果想要项目变得可维护，就需要将 JS 脚本分成不同的独立文件。而需要手动维护大量的 JS 文件是非常麻烦的，因为首先，需要在页面中插入所需要的脚本，其次需要保留正确的`script`标签顺序，使得它们之间的依赖关系可以很好的解决。

## 模块化的发展

对于上面的这些问题，亟需一些解决方案，那就是进行模块化，而 JS 的模块化也经历了比较长的一个发展阶段。

### 直接定义依赖关系

**直接定义依赖关系（Directly Defined Dependencied，DDD）**模式，是首次尝试在 JS 中引入模块的结构，并且也是分离依赖定义的首次实现。

实现这个模式主要是 Dojo Toolkit。Dojo 通过显式地调用`dojo.require`来获取模块代码，以实现直接定义依赖项。这个方法是在需要使用代码的地方，直接定义依赖关系。下面是一个例子：

```js
// greeting.js
dojo.provide("app.greeting")

app.greeting.helloInLang = {
  en: 'hello world',
  es: '¡Hola mundo!'
}

app.greeting.sayHello = function(lang) {
  return app.greeting.helloInLang[lang]
}

// hello.js
dojo.provide("app.hello")
dojo.require("app.greeting")

app.hello = function(x) {
  document.write(app.greeting.sayHello('en'))
}
```

在上面的例子中，使用`dojo.provide`方法来定义模块，当使用`dojo.require`方法时，则是获取模块的代码。

### 命名空间模式

为了解决命名冲突的问题，可以使用特殊的代码约定。如可以给所有的变量和方法添加指定的前缀，或者可以利用函数是 JS 中第一类公民的特性，通过属性-方法的方式创建对象。

Bindows 是首个使用**命名空间**模式组织代码的库。它使用了一个全局对象，这个对象的属性包含了这个库的数据和逻辑，从而代替了对变量或方法使用前缀的命名方式。这种方法也大大地减少了对全局作用域的污染。

下面是一个例子：

```js
// app.js
var app = {}

app.helloInLang = {
  en: 'hello world',
  es: '¡Hola mundo!'
}

// hello.js
app.writeHello = funciton(lang) {
  document.write(app.helloInLang[lang])
}
```

### 模块模式

命名空间虽然给代码组织提供了一定的规则，但是明显还不够，例如它对代码和数据的隔离并没有解决办法。

解决代码数据隔离问题的先驱是**模块模式**。它的主要思想是通过一个闭包来封装数据和代码，然后提供一个外部可访问的方法来访问它们。下面是一个例子：

```js
var greeting = (function() {
  var module = {}
  
  var helloInLang = {
    en: 'hello world',
    es: '¡Hola mundo!'
  }
  
  module.getHello = function(lang) {
    return helloInLang[lang]
  }
  
  module.writeHello = function(lang) {
    document.write(module.getHello(lang))
  }
  return module
})
```

上面的例子中定义了一个立即调用函数，它的返回值是一个 module 对象，这个对象中有一个`getHello`方法，他可以通过闭包来访问`helloInLang`对象。`helloInLang`对象在外部是不可访问的，因此得到了一段原子代码，从而避免了命名冲突。

### 模板定义依赖项

模板定义依赖是 DDD 家族中的下一个模式。最早用于`Prototype 1.4`库中，而 Prototype 库则是 Ruby on Rails 中的一部分。所以模板定义依赖也是从 Ruby on Rails 中演化而来的。

这个模式主要是通过包含特殊标签的目标文件来定义依赖项，然后通过模板化和特殊的构建工具将标签解析成实际的代码。与之前其他的 DDD 模板不同，这个模式只能在预构建步骤工作。

下面是一个例子：

```js
// app.tmp.js
/*borschik:include:../lib/main.js*/
/*borschik:include:../lib/helloInLang.js*/
/*borschik:include:../lib/writeHello.js*/

// main.js
var app = {}

// helloInLang.js
app.helloInLang = {
  en: 'hello world',
  es: '¡Hola mundo!'
}

// writeHello.js
app.writeHello = function(lang) {
  document.write(app.helloInLang[lang])
}
```

在上面的例子中，app.tmp.js 定义了内置脚本和它们的顺序。使用其他标签的 js 文件代替了`script`标签，这个方法的主要目的是为许多其他脚本创建一个文件来进行管理。

### 注释定义依赖项

注释定义依赖项也是 DDD 家族中的一员。它与直接定义依赖项比较相似，不同的是，这个模式是通过使用注释，注释中包含特定模块的所有依赖信息。

使用这个模式的应用必须是预构建的，或者动态解析下载的代码并在运行时解析依赖项。

下面是一个例子：

```js
// helloInLang.js
var helloInLang= {
  en: 'hello world',
  es: '¡Hola mundo!'
}

// sayHello.js
/*! lazy require scripts/app/helloInLang.js */
function sayHello(lang) {
  return helloInLang[lang]
}

// hello.js
/*! lazy require scripts/app/sayHello.js */
document.write(sayHello('en'))
```

它的工作原理是：当库下载文件时，会解析其中的内容，寻找具有依赖项的对应的注释，然后下载依赖项，一直对下载文件重复这个解析过程。

### 外部定义依赖

外部定义依赖是 DDD 家族中的最后一个模式。它将所有的依赖定义在主上下文之外，例如在一个配置文件中或在代码中作为对象或具有依赖性列表的数组。这个模式有一个准备阶段，在这个阶段应用将会以正确的顺序加载所有的依赖项，然后初始化它们。

下面是一个例子：

```js
// deps.json
{
  "files": {
    "main.js": ["sayHello.js"],
    "sayHello.js": ["helloInLang.js"],
    "helloInLang.js": []
  }
}

// helloInLang.js
var helloInLang = {
  en: 'hello world',
  es: '¡Hola mundo!'
}

// sayHello.js
function sayHello(lang) {
  return helloInLang[lang]
}

// main.js
console.log(sayHello('en'))
```

`deps.json`是一个定义了所有依赖的外部上下文文件，当应用启动的时候，loader 会收到这个文件，读取里面作为数组的所有依赖，然后把它们以正确的顺序放在页面里。

### Sandbox Pattern

