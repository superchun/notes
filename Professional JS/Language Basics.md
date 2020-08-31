## 声明

`let` 为 block scoped，`var` 为 function scoped。

```javascript
if (true) {
    var name = 'CC';
    console.log(name); // CC
}
console.log(name); // CC

if (true) {
    let age = 25;
    console.log(age); // 25
}
console.log(age); // ReferenceError: age is not defined

```

`let` 不允许在同一block scoped 中重复声明：

```javascript
var name;
var name;

let age;
let age; // SyntaxError: identifier 'age' has already beed declared
```

**声明风格及其最佳实践**

* 不要使用 `var`
* 比起 `let`，优先使用 `const`

## 数据类型

ES 中有 6 种简单的数据类型，也叫原始类型。分别是：`Undefined`, `Null`,  `Boolean`,  `Number` , `String`,  `Symbol`。

### Undefined 与 Null 区别

如果一个变量使用 `var` 或者 `let` 声明但没有初始化，那么它将被分配为一个 `undefined` 值。

一个 `null` 值是一个空对象指针，当对一个 `null` 值使用 `typeof` 时，将返回 `"object"`：

```javascript
let car = null;
console.log(typeof car); // "object"
```

值 `undefined` 是 `null` 的一个派生。但它们有不同的用法：对于一个变量而言，它的值永远不需要被显式地设为 `undefined`。但这对于 `null` 来说不成立，任何时候一个值期望稍后被使用，但目前不可用时，都应该使用 `null` 来代替。

### Number 类型

#### NaN

`NaN` 是*Not a Number* 的缩写，它是一个特殊的数值，被用于当一个操作需要返回一个数字，但却失败了的情况。例如：在 ES 中，一个数除以 0 将会返回 `NaN`。它有两个属性：

1. 任何包含 `NaN` 的操作都会返回 `NaN`。

2. `NaN` 不等于任何值，包括 `NaN` 自身。

基于第二个属性，ES 提供了 `isNaN()` 方法。

#### Number 转换

 可以使用 `Number()` 函数将各种类型的数据转换为数字。但需要处理 String 的转换时，`parseInt()` 函数比 `Number()` 更好。它们之间有着这样的区别：如果第一个字符不是数字或者正负号，那么 `parseInt()` 将会返回 NaN，`Number()` 则会返回 0。如果第一个字符是数字或者正负号，那么`parseInt()` 的转换会一直持续到 string 结束或者发现了非数字的值。例如：

```javascript
let num1 = parseInt("1234blue"); // 1234
let num2 = parseInt("22.5"); // 22
```

