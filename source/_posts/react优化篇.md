---
title: react优化篇
date: 10/28/2023, 4:09:17 PM
tags: 前端
categories: 前端
---

<!--more-->

### 1.useState传入函数
> &nbsp;&nbsp;useState传入普通变量，每次组件更新都会执行，useState传入函数，只在组件渲染时执行一次，适合数据结构复杂，计算成本高的场景。
##### 举个例子

```tsx
import React, { memo, useState } from "react";
import type { FC, ReactNode } from "react";

interface IProps {
  children?: ReactNode;
}

function genArr() {
  console.log("genArr...");
  const arr = new Array(100);
  arr.fill("hello");
  return arr;
}
const UserStateFnDemo: FC<IProps> = () => {
  console.log("demo...");

  //   const [arr, setArr] = useState(["a", "b", "c"]);
  const [arr, setArr] = useState(genArr);
  function addArr() {
    setArr([...arr, "hello"]);
  }
  return (
    <>
      <div>arr:{arr.length}</div>
      <button onClick={addArr}>公主请点击</button>
    </>
  );
};
export default memo(UserStateFnDemo);
```
如果useState只是传入一个数组，那么组件每次更新的时候，useState都会执行一次，如果给useState传入一个函数，那么useState只在组件渲染完成时执行一次。
### 2.useMemo和memo比较
> &nbsp;&nbsp;`useMemo`用于在函数组件内部进行记忆化计算，返回计算结果。它依赖于指定的依赖项数组，只有依赖项发生变化时才会重新计算。<br/>
> &nbsp;&nbsp;`memo`用于优化组件的渲染，将组件包裹起来返回一个新的优化后的组件。它通过浅比较 `props` 的方式来判断是否重新渲染组件。
##### 举个例子
使用useMemo 1.依赖项是否经常变化 2.缓存元素是否创建成本较高

```tsx
 const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```
`useMemo`是一个React的自定义钩子（hook），用于在函数组件内部进行记忆化计算。它接收一个计算函数和依赖数组作为参数，并返回计算结果。在组件重新渲染时，`useMemo`会根据依赖数组的变化来判断是否重新计算。只有当依赖项发生变化时，`useMemo`才会重新计算并返回新的值，否则它会返回上一次计算的结果。这样可以避免在每次渲染时重复计算相同的值，从而提高性能。<br>

使用memo

```tsx
import React, { memo, useState } from "react";
import type { FC, ReactNode } from "react";

interface IProps {
  children?: ReactNode;
}
const Home: FC<IProps> = () => {
  const [count, setCount] = useState(0);
  const [arr, setArr] = useState(["a", "b"]);
  function addCount() {
    setCount(count + 1);
  }
  function addArr() {
    setArr([...arr, "hello"]);
  }
  console.log("父组件重新渲染");
  return (
    <>
      <div>Home</div>
      Arr: {arr} <br></br>
      <button onClick={addCount}>add</button>
      <button onClick={addArr}>hello</button>
      <Count count={count} />
    </>
  );
};
export default memo(Home);

interface AProps {
  children?: ReactNode;
  count?: number;
}
const Count: FC<AProps> = memo((props: AProps) => {
  const { count } = props;
  console.log("子组件渲染");

  return <div>count:{count}</div>;
});

```
如果不用memo的话点击两个按钮，Count组件都会重新渲染，如果使用memo包裹Count组件，那么只有props发生变化时，Count组件才会重新渲染。
### 3.useCallback
> &nbsp;&nbsp;首先，假如我们不使用useCallback，在父组件中创建了一个名为handleClick的事件处理函数，根据需求我们需要把这个handleClick传给子组件，当父组件中的一些state变化后（这些state跟子组件没有关系），父组件会reRender，然后会重新创建名为handleClick函数实例，并传给子组件，这时即使用React.memo把子组件包裹起来，子组件也会重新渲染，因为props已经变化了，但这个渲染是无意义的

> 如何优化呢？这时候就可以用useCallback了，我们用useCallback把函数包起来之后，在父组件中只有当deps变化的时候，才会创建新的handleClick实例，子组件才会跟着reRender（注意，必须要用React.memo把子组件包起来才有用，否则子组件还是会reRender。React.memo是类似于class组件中的Pure.Component的作用）
##### 举个例子

```jsx
import React, { useState, useCallback } from 'react';

const Parent = () => {
  const [count, setCount] = useState(0);

  const handleClick = useCallback(() => {
    console.log('Button clicked');
    // 其他逻辑...
  }, []);

  return (
    <div>
      <button onClick={handleClick}>Click me</button>
      <Child count={count} handleClick={handleClick} />
    </div>
  );
};

const Child = React.memo(({ count, handleClick }) => {
  console.log('Child 组件渲染');

  return (
    <div>
      <div>Count: {count}</div>
      <button onClick={handleClick}>Click me too</button>
    </div>
  );
});
```
在上述示例中，`handleClick` 函数被传递给了子组件 `Child`。使用 `useCallback` 包裹 `handleClick` 函数，确保它只在依赖项变化时才会重新创建。这样，即使父组件重新渲染，`handleClick` 函数也不会被重新创建，避免了不必要的函数传递和子组件的重新渲染。

总而言之，`useCallback` 可以用于优化函数的创建和传递，确保在依赖项不变的情况下，函数不会被重新创建。这在需要将函数作为 prop 传递给子组件或作为依赖项传递给其他钩子时非常有用。
### 4.优化代码体积
&nbsp;&nbsp;分析代码体积 Analyzing-the-bundle-size

```
npm install --save source-map-explorer
```
然后在`package.json`的scripts模块中添加

```
"analyze": "source-map-explorer 'build/static/js/*.js'"
```
然后终端输入`npm run build`和`npm run analyze`<br>
浏览器显示如下：

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ca142117f57149a4bf41d1f89018cde4~tplv-k3u1fbpfcp-jj-mark:0:0:0:0:q75.image#?w=1909&h=916&s=283589&e=png&b=fdfdfd)
发现首页加载缓慢的主要原因是antd和recharts，也就是Edit组件和Stat组件体积过大，所以我们需要用路由懒加载，才分bundle，优化首页体积。<br>
在router下的index.tsx中

```ts
import { lazy } from "react";

const Edit = lazy(() => import("../pages/Question/Edit"));
const Stat = lazy(() => import("../pages/Question/Stat"));
```
之后我们再在终端输入`npm run build`和`npm run analyze`，浏览器显示如下：

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/36e81621afb945ea9da47ba4793528c1~tplv-k3u1fbpfcp-jj-mark:0:0:0:0:q75.image#?w=1900&h=920&s=247523&e=png&b=fafafa)
main的提及明显变小了，我们继续观察，发现main的体积主要是antd和react-dom，所以我们继续`抽离公共代码，合理利用缓存`。<br>
我们在craco.config.js中写入如下配置:

```js
module.exports = {
  webpack:{
   configure(webpackConfig){
     if(webpackConfig.mode === 'production'){
       //抽离公共代码，只在生产环境
       if(webpackConfig.optimization == null){
         webpackConfig.optimization = {}
       }
       webpackConfig.optimization.splitChunks = {
         chunks:'all',
         cacheGroups:{
           antd:{
             name:'antd-chunk',
             test:/antd/,
             priority:100
           },
           reactDom:{
             name:'reactDom-chunk',
             test:/react-dom/,
             priority:99,
           },
           vendors:{
             name:'vendors-chunk',
             test:/node_modules/,
             priority:98,
           },
         },
       }
     }
     return webpackConfig
   }
  }
}
```
这样就将antd,reactDom,和其他的第三方插件分离成了三个包。<br>
之后我们再在终端输入`npm run build`和`npm run analyze`，浏览器显示如下：

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/afc63740b3b84652b28609b453481472~tplv-k3u1fbpfcp-jj-mark:0:0:0:0:q75.image#?w=1908&h=892&s=278609&e=png&b=fcfcfc)
由图可知main变成了35.03KB。这样我们就完成了优化。
### 5.拆分CSS
&nbsp;&nbsp;CRA根据路由懒加载自动拆分CSS文件。

以上我们主要是通过`缓存数据，减少计算`和`代码分析和拆分，优化首页代码体积`来性能优化的。同时我们优化要根据实际情况，不要先行优化。