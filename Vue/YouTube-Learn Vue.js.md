Vue 中两种数据绑定：

* {{ 属性 }}
*  v-model="属性"

数据绑定是双向的。

Vue 中的条件判断：`v-if="条件表达式"`，同样地，也还有`v-else-if`和`v-else`。如果其中的条件表达式为 false，那么其元素将不会在 DOM 中渲染。

`v-show`也用于 Vue 中的条件判断，与`v-if`不同的是，即使其条件表达式为 false，元素还是会在 DOM 中渲染，只是 CSS 中 display 属性为 none。

`v-bind`后面可以接 HTML 属性，HTML 属性后面可以接一个 JS 条件表达式，用于绑定事件。有时，`v-bind`可以省略，直接冒号`:`后面接 HTML 属性。例子：

```html
<button onclick="alert("Hello world")" v-bind:disabled="input.length < 2">
  	Show
</button>
// 或者
<button onclick="alert("Hello world")" :disabled="input.length < 2">
  	Show
</button>
```

`v-on`用于添加方法，`v-on`后面接方法名，在 Vue 实例中的`methods`对象里面写具体实现。`v-on`的缩写为`@`。例：

```html
<button v-on:click="add">
   add
</button>
// 或者
<button @click="add">
   add
</button>
new Vue({
	...
	methods: {
		add: function() {
			...
		}
	}
})
```



