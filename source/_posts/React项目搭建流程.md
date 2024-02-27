---
title: React项目搭建流程
date: 10/27/2023, 8:13:59 PM
tags: 前端
categories: 前端
---

<!--more-->

## 创建react项目
#### 工具：React脚手架
 create-react-app<br>
 同时配置TypeScript的支持<br>
`create-react-app 项目名称 --template typescript`
## 基本配置
#### 配置项目的icon
#### 配置项目的标题
#### 配置项目的别名
需要下载craco,同时还需要注意**版本问题**<br>
`npm i @craco/craco@alpha -D`<br>
创建craco.config.js文件

![QQ图片20230906180913.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/27acfa0afc7f435596d43d71dec0cc27~tplv-k3u1fbpfcp-jj-mark:0:0:0:0:q75.image#?w=733&h=473&s=45697&e=png&b=1e1e1e)
同时还需注意在tsconfig.json文件中的配置

![QQ图片20230906181046.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e739be82fe5746fc8ef6c4f4610171a2~tplv-k3u1fbpfcp-jj-mark:0:0:0:0:q75.image#?w=1259&h=956&s=199148&e=png&b=232323)
#### 配置tsconfig.json
## 对项目结构进行划分

![QQ图片20230906181303.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9c7604ffb39643f28a9267f8c73d3fb6~tplv-k3u1fbpfcp-jj-mark:0:0:0:0:q75.image#?w=266&h=813&s=47404&e=png&b=262627)
## 对默认css样式进行重置
#### normalize.css
`npm i normalize.css`
#### reset.less
在react中使用less<br>
`npm i craco-less@2.1.0-alpha.0`

在craco.config.js中的配置为

![QQ图片20230906181731.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/04d422c9fcb04a819fb3686ba20badc5~tplv-k3u1fbpfcp-jj-mark:0:0:0:0:q75.image#?w=743&h=467&s=45422&e=png&b=1e1e1e)
## 路由的配置
#### 安装路由
`npm i react-router-dom`

创建index.tsx文件

```tsx
import React,{lazy} from 'react'
import {Navigate,RouteObject} from 'react-router-dom'
//路由的懒加载
const Discover = lazy(()=>import('@/views/discover'))
const Album = lazy(() => import('@/views/discover/c-views/album'))
const Recommend = lazy(() => import('@/views/discover/c-views/recommend'))
const routes:RouteObject[] = [
{
  path:'/',
  element:<Navigate to="/discover"/>
},
  {
    path: '/discover',
    element: <Discover />,
    children: [
      {
        path: '/discover',
        element: <Navigate to="/discover/recommend" />
      },
      {
        path: '/discover/album',
        element: <Album />
      }
    ]
  },
]
export default routes
```
#### 在组件中使用路由

```tsx
        <Link to="/discover">发现音乐</Link>
        <Link to="/mine">我的音乐</Link>
        <Link to="focus">关注</Link>
        <Link to="download">下载客户端</Link>
```
使用useRoutes这个hook

```tsx
        <Suspense fallback="">
          <div className="main">{useRoutes(routes)}</div>
        </Suspense>
```
在index.tsx中

```tsx
import { HashRouter } from 'react-router-dom'
const root = ReactDOM.createRoot(document.getElementById('root') as HTMLElement)
root.render(
    <HashRouter>
      <App />
    </HashRouter>
)

```

## redux的配置
#### 安装
我们以使用toolkit工具为例<br>
`npm i @reduxjs/toolkit react-redux`

创建store/index.ts文件

```ts
import {configureStore} from '@reduxjs/toolkit'

const store = configureStore({
  reducer:{}
})

export default store
```
在index.tsx中使用store

```tsx
  <Provider store={store}>
   <HashRouter>
    <App/>
   </HashRouter>
   </Provider>
```
创建modules文件夹 以创建一个counter.ts文件为例<br>

```ts
import {createSlice} from '@reduxjs/toolkit'
const counterSlice = createSlice({
  name:'counter',
  initialState:{
   count:0
   },
   reducers:{}
})
export default counterSlice.reducer
```
然后我们可以试着在App.tsx中获取一下counter中的数据<br>

```tsx
import {useSelector,shallowEqual} from 'react-redux'
//定义state的类型
type GetStateFnType = typeof store.getState
type IRootState = ReturnType<GetStateFnType>
fuction App(){
const {count} = useSelector(
(state:IRootState)=>({
  count:state.counter.count
}),
shallowEqual
)
...
}
```
但是我们可以进一步优化一下代码，将定义state的类型代码写入store中，同时我们可以创建一个useAppSelector这样的hook函数，这样我们使用它代替useSelector，其中的state类型就会自行推断了。

```store/index.ts
import { useSelector, TypedUseSelectorHook } from 'react-redux'
...
type GetStateFnType = typeof store.getState
type IRootState = ReturnType<GetStateFnType>
//useAppSelector的hook
export const useAppSelector:TypeUseSelectorHook<IRootState>=useSelector
...
```
这样在App.tsx文件中我们只需要将useSelector函数换成useAppSelector函数即可

```App.tsx
import { shallowEqual } from 'react-redux'
import { useAppSelector } from './store'
...
function App() {
  const { count, message } = useAppSelector(
    (state) => ({
      count: state.counter.count,
      message: state.counter.message
    }),
    shallowEqual
  )
  ...
}
```






