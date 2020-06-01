## Basic

用 `.` 开头作为 `class` 的选择器

用 `#` 开头作为 `id` 的选择器

`class` 可以复用，`id` 只能唯一

## Structure

HTML 以文件树的方式作为其结构，其中有 `parent` 和 `child` 关系、`ancestor` 和 `descendand` 关系、`adjacent sibling` 关系。

以空格连接的方式来表示后代选择器，如：

```css
h1 em {color: gray;}
```

上面的写法表示只对 h1 后代中的 em 进行 layout。

以大写符号 `>` 的方式来表示子选择器，如：

```css
h1 > strong {color: red;}
```

上面的写法表示只对 h1 的直接子元素进行 layout。

用 `+` 表示相邻兄弟选择器，如：

```css
h1 + p {margin-top: 0;}
```

上面的写法表示选择跟在 h1 元素后面的 p 元素，且 h1 和 p 元素有共同的父元素。

