# Vue 实例

## 创建一个 Vue 实例

每个 Vue 应用都是通过`Vue`函数创建一个新的 **Vue 实例** 开始的。一个 Vue 应用由一个通过`new Vue`创建的**根实例**，以及可选的、嵌套的、可复用的组件树组成。

```js
var vm = new Vue({

})
```

## 数据与方法

当一个 Vue 实例被创建时，它将`data`对象中所有的 property 加入到 Vue 的**响应式系统**中。当这些 property 的值发生改变时，视图将会更新为新的值。

只有当实例被创建时就已经存在于`data`中的 property 才是响应式的。使用`Object.freeze()`会阻止修改现有的 property，响应系统无法再追踪变化。

```js
var data = {a: 1}
var vm = new Vue({
    data: data
})
vm.a == data.a // true

vm.a = 2
data.a // => 2

// 后加入的 property 不会触发任何视图的更新
vm.b = 'hi'

```

Vue 实例暴露了一些有用的实例 property 与方法，它们都有前缀 `$`，以方便和用户定义的 property 区分开。

## 生命周期钩子

每个 Vue 实例被创建时都要经过一系列的初始化过程，同时在这个过程也会运行一些叫做**生命周期钩子**的函数，给用户在不同阶段添加自己的代码的机会。

比如`created`钩子可以用来在一个实例被创建后执行代码：

```js
new Vue({
	data: {
		a: 1
	},
	created: function() {
		console.log('a is ' + this.a)
	}
})
// => "a is 1"
```

下面是生命周期的图示：

<img src="https://cn.vuejs.org/images/lifecycle.png" alt="Vue 实例生命周期" style="zoom:50%;" />