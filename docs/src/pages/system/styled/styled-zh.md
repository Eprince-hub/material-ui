# `styled()`

<p class="description">创建样式组件(styled components)的工具</p>

## 简介

All the MUI components are styled with this `styled()` utility. This utility is built on top of the `styled()` module of [`@mui/styled-engine`](/guides/styled-engine/) and provides additional features.

### 引用路径

You can use the utility coming from the `@mui/system` package, or if you are using `@mui/material`, you can import it from `@mui/material/styles`. 区别是如果React Context 上下文中没有可以用的主题的情况下，后者会自带默认的主题 `theme`。

### 这解决了什么问题？

该能力可以作为 emotion 或者是styled-components 中styled()能力的替代品 处理的都是一样的问题，但是它额外提供了下面这些内容

1. It uses MUI's default `theme` if no theme is available in React context.
2. 支持根据`name`属性设置主题中的[`styleOverrides`](/customization/theme-components/#global-style-overrides) 和 [`variants`](/customization/theme-components/#adding-new-component-variants)（可忽略）。
3. 新增对`sx`属性的支持(可忽略）
4. It adds by default `shouldForwardProp` option that is taking into account all props used internally in the MUI components (can be overridden).

## API

### `styled(Component, [options])(styles) => Component`

#### 参数

1. `Component` ：将被包装的组件。
2. `options` (_object_ [optional]):

   - `options.shouldForwardProp` (_`(prop: string) => bool`_ [optional]): Indicates whether the `prop` should be forwarded to the `Component`.
   - `options.label` (_string_ [optional]): css样式后缀 适合调试。
   - `options.name` (_string_ [optional]): 会根据此属性在`theme.components` 中找到相应的`styleOverrides` and `variants`并应用到组件上 同时也会用来生成 `label`
   - `options.slot` (_string_ [optional]):如果是`Root`, 才会自动装配主题上对应`name`下的的`styleOverrides` & <0>variants</0>.
   - `options.overridesResolver` (_(props: object, styles: Record<string, styles>) => styles_ [optional]): Function that returns styles based on the props and the `theme.components[name].styleOverrides` object.
   - `options.skipVariantsResolver` (_bool_): 不再自动装配 `theme.components[name].variants`
   - `options.skipSx` (_bool_ [optional]): 该组件禁用`sx`属性
   - 其它选项会透传到emotion's `styled([Component], [options])` 的 [`options`](https://emotion.sh/docs/styled) 参数.

#### 返回结果

`Component` ：已创建的新组建。

## 基本用法

{{"demo": "pages/system/styled/BasicUsage.js", "defaultCodeOpen": true}}

## 使用主题

{{"demo": "pages/system/styled/ThemeUsage.js", "defaultCodeOpen": true}}

## 自定义组件

下面演示下如何通过 `styled` API 来创建一个包含源组件原有功能的自定义组件

{{"demo": "pages/system/styled/UsingOptions.js", "defaultCodeOpen": true }}

If you inspect this element with the browser DevTools in development mode, you will notice that the class of the component now ends with the `MyThemeComponent-root`, which comes from the `name` and `slot` options that were provided.

<img src="/static/images/system/styled-options.png" alt="browser DevTools showing the rendered component" width="327" />

In addition to this, the `color`, `sx`, and `variant` props are not propagated to the generated `div` element.

## 如何禁用

If you would like to remove some of the MUI specific features, you can do it like this:

```diff
 const StyledComponent = styled('div', {}, {
   name: 'MuiStyled',
   slot: 'Root',
-  overridesResolver: (props, styles) => styles.root, // disables theme.components[name].styleOverrides
+  skipVariantsResolver: true, // disables theme.components[name].variants
+  skipSx: true, // disables the sx prop
 });
```

## 创建自定义的 `styled()`工具

If you want to have a different default theme for the `styled()` utility, you can create your own version of it, using the `createStyled()` utility.

```js
import { createStyled, createTheme } from '@mui/system';

const defaultTheme = createTheme({
  // your custom theme values
});

const styled = createStyled({ defaultTheme });

export default styled;
```

## Difference with the `sx` prop

The `styled` function is an extension of the `styled` utility provided by the underlying style library used – either emotion or styled-components. It is guaranteed that it will produce the same output as the `styled` function coming from the style library for the same input.

The [`sx`](/system/the-sx-prop/) prop, on the other hand, is a new way of styling your components, focused on fast customization. `styled` is a function, while `sx` is a prop of the MUI components.

Therefore, you will notice the following differences:

### `sx` provides more shortcuts than `styled`

With `styled`:

```js
const MyStyledButton = styled('button')({
  mx: 1, // ❌ don't use this! This shortcut is only provided by the `sx` prop
});
```

With `sx`:

```js
const MyStyledButton = (props) => (
  <button sx={{
    mx: 1, // ✔️ this shortcut is specific to the `sx` prop,
  }}>
     {props.children}
  </button>
})
```

### The style definition varies slightly

With `styled`:

```js
const MyStyledButton = styled('button')({
  padding: 1, // means "1px", NOT "theme.spacing(1)"
});
```

With `sx`:

```js
const MyStyledButton = (props) => (
  <button sx={{
    padding: 1 // means "theme.spacing(1)", NOT "1px"
  }}>
     {props.children}
  </button>
})
```

### Patterns for how to use props differ

With `styled`:

```js
const MyStyledButton = styled('button')((props) => ({
  backgroundColor: props.myBackgroundColor,
}));
```

With `sx`:

```js
const MyStyledButton = (props) => (
  <button sx={{backgroundColor: props.myCustomColor}}>
     {props.children}
  </button>
})
```

### Parameter when using function are different for each field

With `styled` (not recommended):

```js
// You may find this syntax in the wild, but for code readability
// we recommend using only one top-level function
const MyStyledButtonPropsPerField = styled('button')({
  backgroundColor: (props) => props.myBackgroundColor,
});
```

With `sx`:

```js
import { lighten } from "polished"
const MyStyledButton = (props) => (
  <button sx={{backgroundColor: theme => lighten(0.2,theme.palette.primary.main)}}>
     {props.children}
  </button>
})
// Note: for direct theme access without modification, you can also use a shortcut by providing the key as a string
const MyStyledButton = (props) => (
  <button sx={{backgroundColor: "primary.main"}}>
     {props.children}
  </button>
})
```

### How can I use the `sx` syntax with the `styled()` utility?

If you are one of those who prefers the `sx` syntax and wants to use it in both the `sx` prop and the `styled()` utility, you can use the `experimental_sx` utility from the `@mui/system`:

{{"demo": "pages/system/styled/UsingWithSx.js", "defaultCodeOpen": true}}

The overhead added by using the `experimental_sx` utility is the same as if you were to use the `sx` prop on the component.

> Note: You can use `experimental_sx` outside of the `styled()` utility, too; e.g., for defining `variants` in your custom theme.

## How to use components selector API

If you've ever used the `styled()` API of either [`emotion`](https://emotion.sh/docs/styled#targeting-another-emotion-component) or [`styled-components`](https://styled-components.com/docs/advanced#referring-to-other-components), you should have been able to use components as selectors.

```jsx
import styled from '@emotion/styled';

const Child = styled.div`
  color: red;
`;

const Parent = styled.div`
  ${Child} {
    color: green;
  }
`;

render(
  <div>
    <Parent>
      <Child>Green because I am inside a Parent</Child>
    </Parent>
    <Child>Red because I am not inside a Parent</Child>
  </div>,
);
```

With MUI's `styled()` utility, you can use components as selectors, too. When using `@mui/styled-engine-sc` (`styled-components`), nothing needs to be done. When using `@mui/styled-engine` (`emotion`), the default engine, there are a few steps you should perform:

First, you should install [`@emotion/babel-plugin`](https://emotion.sh/docs/@emotion/babel-plugin).

```sh
npm install @emotion/babel-plugin
```

Then, configure the plugin to know about the MUI version of the `styled()` utility:

**babel.config.js**

```js
module.exports = {
  ...
  plugins: [
    [
      "@emotion",
      {
        importMap: {
          "@mui/system": {
            styled: {
              canonicalImport: ["@emotion/styled", "default"],
              styledBaseImport: ["@mui/system", "styled"]
            }
          },
          "@mui/material/styles": {
            styled: {
              canonicalImport: ["@emotion/styled", "default"],
              styledBaseImport: ["@mui/material/styles", "styled"]
            }
          }
        }
      }
    ]
  ]
};

```

Now you should be able to use components as your selectors!
