软件的设计是为了服务理念。只有懂了设计理念，才能明白为了实现这样的理念需要如何架构。
所以，在我们深入源码架构之前，先来聊聊`React`理念。

## React 理念

我们可以从[官网](https://zh-hans.reactjs.org/docs/thinking-in-react.html)看到`React`的理念：

> 我们认为，React 是用 JavaScript 构建**快速响应**的大型 Web 应用程序的首选方式。它在 Facebook 和 Instagram 上表现优秀。

可见，关键是实现`快速响应`。那么制约`快速响应`的因素是什么呢？
我们日常使用 App，浏览网页时，有两类场景会制约`快速响应`：

- 当遇到大计算量的操作或者设备性能不足使页面掉帧，导致卡顿。**（CPU 的瓶颈）**
- 发送网络请求后，由于需要等待数据返回才能进一步操作导致不能快速响应。**（IO 的瓶颈）**

`React`是如何解决这两个瓶颈的呢？

## CPU 的瓶颈

当项目变得庞大、组件数量繁多时，就容易遇到 CPU 的瓶颈。
考虑如下 Demo，我们向视图中渲染 3000 个`li`：

```
function App() {
  const len = 3000;
  return (
    <ul>
      {Array(len)
        .fill(0)
        .map((_, i) => (
          <li>{i}</li>
        ))}
    </ul>
  );
}

const rootEl = document.querySelector("#root");
ReactDOM.render(<App />, rootEl);
```

主流浏览器刷新频率为 60Hz，即每（1000ms / 60Hz）16.6ms 浏览器刷新一次。
我们知道，JS 可以操作 DOM，`GUI渲染线程`与`JS线程`是互斥的。所以**JS 脚本执行**和**浏览器布局、绘制**不能同时执行。

在每 16.6ms 时间内，需要完成如下工作：

```
JS脚本执行 -----  样式布局 ----- 样式绘制
```

当 JS 执行时间过长，超出了 16.6ms，这次刷新就没有时间执行**样式布局**和**样式绘制**了。

在 Demo 中，由于组件数量繁多（3000 个），JS 脚本执行时间过长，页面掉帧，造成卡顿。
可以从打印的执行堆栈图看到，JS 执行时间为 73.65ms，远远多于一帧的时间。

![长任务](https://react.iamkasong.com/img/long-task.png)

如何解决这个问题呢？

答案是：在浏览器每一帧的时间中，预留一些时间给 JS 线程，`React`利用这部分时间更新组件（可以看到，在[源码](https://github.com/facebook/react/blob/1fb18e22ae66fdb1dc127347e169e73948778e5a/packages/scheduler/src/forks/SchedulerHostConfig.default.js#L119)中，预留的初始时间是 5ms）。
当预留的时间不够用时，`React`将线程控制权交还给浏览器使其有时间渲染 UI，`React`则等待下一帧时间到来继续被中断的工作。

> 这种将长任务分拆到每一帧中，像蚂蚁搬家一样一次执行一小段任务的操作，被称为`时间切片`（time slice）

接下来我们开启`Concurrent Mode`（开启后会启用`时间切片`）：

```
// 通过使用ReactDOM.unstable_createRoot开启Concurrent Mode
// ReactDOM.render(<App/>, rootEl);
ReactDOM.unstable_createRoot(rootEl).render(<App />);
```

此时我们的长任务被拆分到每一帧不同的`task`中，`JS脚本`执行时间大体在`5ms`左右，这样浏览器就有剩余时间执行**样式布局**和**样式绘制**，减少掉帧的可能性。

`react`应用的`启动模式`:

在当前稳定版`react@17.0.2`源码中, 有 3 种启动方式. 先引出官网上对于[这 3 种模式的介绍](https://zh-hans.reactjs.org/docs/concurrent-mode-adoption.html#why-so-many-modes), 其基本说明如下:

1. `legacy` 模式: `ReactDOM.render(<App />, rootNode)`. 这是当前 React app 使用的方式. 这个模式可能不支持[这些新功能(concurrent 支持的所有功能)](https://zh-hans.reactjs.org/docs/concurrent-mode-patterns.html#the-three-steps).

   ```
   // LegacyRoot
   ReactDOM.render(<App />, document.getElementById('root'), (dom) => {}); // 支持callback回调, 参数是一个dom对象
   ```

2. [Blocking 模式](https://zh-hans.reactjs.org/docs/concurrent-mode-adoption.html#migration-step-blocking-mode): `ReactDOM.createBlockingRoot(rootNode).render(<App />)`. 目前正在实验中, 它仅提供了 `concurrent` 模式的小部分功能, 作为迁移到 `concurrent` 模式的第一个步骤.

   ```
   // BlockingRoot// 1. 创建ReactDOMRoot对象
   const reactDOMBlockingRoot = ReactDOM.createBlockingRoot(  document.getElementById('root'),);
   // 2. 调用render
   reactDOMBlockingRoot.render(<App />); // 不支持回调
   ```

3. [Concurrent 模式](https://zh-hans.reactjs.org/docs/concurrent-mode-adoption.html#enabling-concurrent-mode): `ReactDOM.createRoot(rootNode).render(<App />)`. 目前在实验中, 未来稳定之后，打算作为 React 的默认开发模式. 这个模式开启了所有的新功能.

   ```
   // ConcurrentRoot
   // 1. 创建ReactDOMRoot对象
   const reactDOMRoot = ReactDOM.createRoot(document.getElementById('root'));
   // 2. 调用render
   reactDOMRoot.render(<App />); // 不支持回调
   ```

注意: 虽然`17.0.2`的源码中有[`createRoot`和`createBlockingRoot`方法](https://github.com/facebook/react/blob/v17.0.2/packages/react-dom/src/client/ReactDOM.js#L202)(如果自行构建, [会默认构建`experimental`版本](https://github.com/facebook/react/blob/v17.0.2/scripts/rollup/build.js#L30-L35)), 但是稳定版的构建入口[排除掉了这两个 api](https://github.com/facebook/react/blob/v17.0.2/packages/react-dom/index.stable.js), 所以实际在`npm i react-dom`安装`17.0.2`稳定版后, 不能使用该 api.如果要想体验非`legacy`模式, 需要[显示安装 alpha 版本](https://github.com/reactwg/react-18/discussions/9)(或自行构建).

通过以上内容，我们可以看到，`React`为了践行“构建**快速响应**的大型 Web 应用程序”理念做出的努力。
其中的关键是解决 CPU 的瓶颈与 IO 的瓶颈。而落实到实现上，则需要将**同步的更新**变为**可中断的异步更新**。

`React`从 v15 升级到 v16 后重构了整个架构。为什么 v15 不能满足**快速响应**的理念，以至于被重构。

## React15 架构

React15 架构可以分为两层：

- Reconciler（协调器）—— 负责找出变化的组件
- Renderer（渲染器）—— 负责将变化的组件渲染到页面上

### Reconciler（协调器）

我们知道，在`React`中可以通过`this.setState`、`this.forceUpdate`、`ReactDOM.render`等 API 触发更新。
每当有更新发生时，**Reconciler**会做如下工作：

- 调用函数组件、或 class 组件的`render`方法，将返回的 JSX 转化为虚拟 DOM
- 将虚拟 DOM 和上次更新时的虚拟 DOM 对比
- 通过对比找出本次更新中变化的虚拟 DOM
- 通知**Renderer**将变化的虚拟 DOM 渲染到页面上

### Renderer（渲染器）

由于`React`支持跨平台，所以不同平台有不同的**Renderer**。我们前端最熟悉的是负责在浏览器环境渲染的**Renderer** —— [ReactDOM](https://www.npmjs.com/package/react-dom)。
除此之外，还有：

- [ReactNative](https://www.npmjs.com/package/react-native)渲染器，渲染 App 原生组件
- [ReactTest](https://www.npmjs.com/package/react-test-renderer)渲染器，渲染出纯 Js 对象用于测试
- [ReactArt](https://www.npmjs.com/package/react-art)渲染器，渲染到 Canvas, SVG 或 VML (IE8)

在每次更新发生时，**Renderer**接到**Reconciler**通知，将变化的组件渲染在当前宿主环境。

## React15 架构的缺点

在**Reconciler**中，`mount`的组件会调用[mountComponent](https://github.com/facebook/react/blob/15-stable/src/renderers/dom/shared/ReactDOMComponent.js#L498)，`update`的组件会调用[updateComponent](https://github.com/facebook/react/blob/15-stable/src/renderers/dom/shared/ReactDOMComponent.js#L877)。这两个方法都会递归更新子组件。

##递归更新的缺点

由于递归执行，所以更新一旦开始，中途就无法中断。当层级很深时，递归更新时间超过了 16ms，用户交互就会卡顿。

**Reconciler**和**Renderer**是交替工作的，由于整个过程都是同步的，所以在用户看来所有 DOM 是同时更新的。无法实现中断，否则会看见更新不完全的 DOM，基于这个原因，`React`决定重写整个架构。

## React16 架构

React16 架构可以分为三层：

- Scheduler（调度器）—— 调度任务的优先级，高优任务优先进入**Reconciler**
- Reconciler（协调器）—— 负责找出变化的组件
- Renderer（渲染器）—— 负责将变化的组件渲染到页面上

### Scheduler（调度器）

既然我们以浏览器是否有剩余时间作为任务中断的标准，那么我们需要一种机制，当浏览器有剩余时间时通知我们。
其实部分浏览器已经实现了这个 API，这就是[requestIdleCallback](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/requestIdleCallback)。但是由于以下因素，`React`放弃使用：

- 浏览器兼容性
- 触发频率不稳定，受很多因素影响。比如当我们的浏览器切换 tab 后，之前 tab 注册的`requestIdleCallback`触发的频率会变得很低

基于以上原因，`React`实现了功能更完备的`requestIdleCallback`polyfill，这就是**Scheduler**。除了在空闲时触发回调的功能外，**Scheduler**还提供了多种调度优先级供任务设置。

### Reconciler（协调器）

我们知道，在 React15 中**Reconciler**是递归处理虚拟 DOM 的。让我们看看[React16 的 Reconciler](https://github.com/facebook/react/blob/1fb18e22ae66fdb1dc127347e169e73948778e5a/packages/react-reconciler/src/ReactFiberWorkLoop.new.js#L1673)。
我们可以看见，更新工作从递归变成了可以中断的循环过程。每次循环都会调用`shouldYield`判断当前是否有剩余时间。

```
/** @noinline */
function workLoopConcurrent() {
  // Perform work until Scheduler asks us to yield
  while (workInProgress !== null && !shouldYield()) {
    workInProgress = performUnitOfWork(workInProgress);
  }
}
```

那么 React16 是如何解决中断更新时 DOM 渲染不完全的问题呢？
在 React16 中，**Reconciler**与**Renderer**不再是交替工作。当**Scheduler**将任务交给**Reconciler**后，**Reconciler**会为变化的虚拟 DOM 打上代表增/删/更新的标记，类似这样：

```
export const Placement = /*             */ 0b0000000000010;
export const Update = /*                */ 0b0000000000100;
export const PlacementAndUpdate = /*    */ 0b0000000000110;
export const Deletion = /*              */ 0b0000000001000;
```

### Renderer（渲染器）

**Renderer**根据**Reconciler**为虚拟 DOM 打的标记，同步执行对应的 DOM 操作。

![image.png](/tencent/api/attachments/s3/url?attachmentid=23014462)

其中红框中的步骤随时可能由于以下原因被中断：

- 有其他更高优任务需要先更新
- 当前帧没有剩余时间

由于红框中的工作都在内存中进行，不会更新页面上的 DOM，所以即使反复中断，用户也不会看见更新不完全的 DOM。

整个**Scheduler**与**Reconciler**的工作都在内存中进行。只有当所有组件都完成**Reconciler**的工作，才会统一交给**Renderer**。

![image.png](/tencent/api/attachments/s3/url?attachmentid=23014871)

## 基础包结构

1. react
   > react 基础包, 只提供定义 react 组件(`ReactElement`)的必要函数, 一般来说需要和渲染器(`react-dom`,`react-native`)一同使用. 在编写`react`应用的代码时, 大部分都是调用此包的 api.
2. react-dom
   > react 渲染器之一, 是 react 与 web 平台连接的桥梁(可以在浏览器和 nodejs 环境中使用), 将`react-reconciler`中的运行结果输出到 web 界面上. 在编写`react`应用的代码时,大多数场景下, 能用到此包的就是一个入口函数`ReactDOM.render(<App/>, document.getElementById('root'))`, 其余使用的 api, 基本是`react`包提供的.
3. react-reconciler
   > react 得以运行的核心包(综合协调`react-dom`,`react`,`scheduler`各包之间的调用与配合).
   > 管理 react 应用状态的输入和结果的输出. 将输入信号最终转换成输出信号传递给渲染器.
   - 接受输入(`scheduleUpdateOnFiber`), 将`fiber`树生成逻辑封装到一个回调函数中(涉及`fiber`树形结构, `fiber.updateQueue`队列, 调和算法等),
   - 把此回调函数(`performSyncWorkOnRoot`或`performConcurrentWorkOnRoot`)送入`scheduler`进行调度
   - `scheduler`会控制回调函数执行的时机, 回调函数执行完成后得到全新的 fiber 树
   - 再调用渲染器(如`react-dom`, `react-native`等)将 fiber 树形结构最终反映到界面上
4. scheduler
   > 调度机制的核心实现, 控制由`react-reconciler`送入的回调函数的执行时机, 在`concurrent`模式下可以实现任务分片. 在编写`react`应用的代码时, 同样几乎不会直接用到此包提供的 api.
   - 核心任务就是执行回调(回调函数由`react-reconciler`提供)
   - 通过控制回调函数的执行时机, 来达到任务分片的目的, 实现可中断渲染(`concurrent`模式下才有此特性)

![image.png](/tencent/api/attachments/s3/url?attachmentid=23099238)

## Fiber 的起源

> 最早的`Fiber`官方解释来源于[2016 年 React 团队成员 Acdlite 的一篇介绍](https://github.com/acdlite/react-fiber-architecture)。

在`React15`及以前，`Reconciler`采用递归的方式创建虚拟 DOM，递归过程是不能中断的。如果组件树的层级很深，递归会占用线程很多时间，造成卡顿。

为了解决这个问题，`React16`将**递归的无法中断的更新**重构为**异步的可中断更新**，由于曾经用于递归的**虚拟 DOM**数据结构已经无法满足需要。于是，全新的`Fiber`架构应运而生。

## Fiber 的含义

`Fiber`包含三层含义：

1. 作为架构来说，之前`React15`的`Reconciler`采用递归的方式执行，数据保存在递归调用栈中，所以被称为`stack Reconciler`。`React16`的`Reconciler`基于`Fiber节点`实现，被称为`Fiber Reconciler`。
2. 作为静态的数据结构来说，每个`Fiber节点`对应一个`React element`，保存了该组件的类型（函数组件/类组件/原生组件...）、对应的 DOM 节点等信息。
3. 作为动态的工作单元来说，每个`Fiber节点`保存了本次更新中该组件改变的状态、要执行的工作（需要被删除/被插入页面中/被更新...）。

```
function FiberNode(
  tag: WorkTag,
  pendingProps: mixed,
  key: null | string,
  mode: TypeOfMode,
) {
  // 作为静态数据结构的属性
  this.tag = tag;
  this.key = key;
  this.elementType = null;
  this.type = null;
  this.stateNode = null;

  // 用于连接其他Fiber节点形成Fiber树
  this.return = null;
  this.child = null;
  this.sibling = null;
  this.index = 0;

  this.ref = null;

  // 作为动态的工作单元的属性
  this.pendingProps = pendingProps;
  this.memoizedProps = null;
  this.updateQueue = null;
  this.memoizedState = null;
  this.dependencies = null;

  this.mode = mode;

  this.effectTag = NoEffect;
  this.nextEffect = null;

  this.firstEffect = null;
  this.lastEffect = null;

  // 调度优先级相关
  this.lanes = NoLanes;
  this.childLanes = NoLanes;

  // 指向该fiber在另一次更新时对应的fiber
  this.alternate = null;
}
```

### 作为架构来说

每个 Fiber 节点有个对应的`React element`，多个`Fiber节点`是如何连接形成树呢？靠如下三个属性：

```
// 指向父级Fiber节点
this.return = null;
// 指向子Fiber节点
this.child = null;
// 指向右边第一个兄弟Fiber节点
this.sibling = null;
```

举个例子，如下的组件结构：

```
function App() {
  return (
    <div>
      i am
      <span>alaia</span>
    </div>
  )
}
```

对应的`Fiber树`结构：
![image.png](/tencent/api/attachments/s3/url?attachmentid=23015362)

### 作为静态的数据结构

作为一种静态的数据结构，保存了组件相关的信息：

```
// Fiber对应组件的类型 Function/Class/Host...
this.tag = tag;
// key属性
this.key = key;
// 大部分情况同type，某些情况不同，比如FunctionComponent使用React.memo包裹
this.elementType = null;
// 对于 FunctionComponent，指函数本身，对于ClassComponent，指class，对于HostComponent，指DOM节点tagName
this.type = null;
// Fiber对应的真实DOM节点
this.stateNode = null;
```

### 作为动态的工作单元

作为动态的工作单元，`Fiber`中如下参数保存了本次更新相关的信息：

```
// 保存本次更新造成的状态改变相关信息
this.pendingProps = pendingProps;
this.memoizedProps = null;
this.updateQueue = null;
this.memoizedState = null;
this.dependencies = null;

this.mode = mode;

// 保存本次更新会造成的DOM操作
this.effectTag = NoEffect;
this.nextEffect = null;

this.firstEffect = null;
this.lastEffect = null;
```

如下两个字段保存调度优先级相关的信息：

```
// 调度优先级相关
this.lanes = NoLanes;
this.childLanes = NoLanes;
```

`Fiber节点`可以构成`Fiber树`。那么`Fiber树`和页面呈现的`DOM树`有什么关系，`React`又是如何更新`DOM`的呢

### ReactElement, Fiber, DOM 三者的关系

这里我们梳理出`ReactElement, Fiber, DOM`这 3 种对象的关系

1. [ReactElement 对象](https://github.com/facebook/react/blob/v17.0.2/packages/react/src/ReactElement.js#L126-L146)(type 定义在[shared 包中](https://github.com/facebook/react/blob/v17.0.2/packages/shared/ReactElementType.js#L15))
   - 所有采用`jsx`语法书写的节点, 都会被编译器转换, 最终会以`React.createElement(...)`的方式, 创建出来一个与之对应的`ReactElement`对象
2. [fiber 对象](https://github.com/facebook/react/blob/v17.0.2/packages/react-reconciler/src/ReactFiber.old.js#L116-L155)(type 类型的定义在[ReactInternalTypes.js](https://github.com/facebook/react/blob/v17.0.2/packages/react-reconciler/src/ReactInternalTypes.js#L47-L174)中)
   - `fiber对象`是通过`ReactElement`对象进行创建的, 多个`fiber对象`构成了一棵`fiber树`, `fiber树`是构造`DOM树`的数据模型, `fiber树`的任何改动, 最后都体现到`DOM树`.
3. [DOM 对象](https://developer.mozilla.org/zh-CN/docs/Web/API/Document_Object_Model): 文档对象模型
   - `DOM`将文档解析为一个由节点和对象（包含属性和方法的对象）组成的结构集合, 也就是常说的`DOM树`.
   - `JavaScript`可以访问和操作存储在 DOM 中的内容, 也就是操作`DOM对象`, 进而触发 UI 渲染.

![image.png](/tencent/api/attachments/s3/url?attachmentid=23015886)

它们之间的关系反映了我们书写的 JSX 代码到 DOM 节点的转换过程：
![image.png](/tencent/api/attachments/s3/url?attachmentid=23015919)

### 双缓冲技术(double buffering)

`ReactElement, Fiber, DOM三者的关系`, `fiber树`的构造过程, 就是把`Res;
this.childLactElemes = NoLant`转换成`fiber树`的过程. 在这个过程中, 内存里会同时存在 2 棵`fiber树`:

- 其一: 代表当前界面的`fiber`树(已经被展示出来, 挂载到`fiberRoot.current`上). 如果是初次构造(`初始化渲染`), 页面还没有渲染, 此时界面对应的 fiber 树为空(`fiberRoot.current = null`).
- 其二: 正在构造的`fiber`树(即将展示出来, 挂载到`HostRootFiber.alternate`上, 正在构造的节点称为`workInProgress`). 当构造完成之后, 重新渲染页面, 最后切换`fiberRoot.current = workInProgress`, 使得`fiberRoot.currement`重新指向代表当前界面的`fiber`树.

此处涉及到 2 个全局对象`fiberRoot`和`HostRootFiber`, 在[React 应用的启动过程](https://7km.top/main/bootstrap)中有详细的说明.
用图来表述`double buffering`的概念如下:

1. 构造过程中, `fiberRoot.curreact`指向当前界面对应的`fiber`树.
   ![image.png](/tencent/api/attachments/s3/url?attachmentid=23019742)
2. 构造完成并渲染, 切换`fiberRook/react.current`指针, 使其继续指向当前界面对应的`fiber`树(原来代表界面的 fiber 树, 变成了内存中).
   ![image.png](/tencent/api/attachments/s3/url?attachmentid=23019757)

## React 工作循环 (workLoop)

在 reaactElement 工作时有两个大的循环, 它们分别位于`scheduler`和`react/blob/v17.0.2/packages/react-reconciler`包中:

![image.png](/tencent/api/attachments/s3/url?attachmentid=23020385)

本文将这两个循环分别表述为`任务调度循环`和`fiber构造循环`

1. `任务调度循环`

源码位于[`Scheduler.js`](https://github.com/facebook/react/blob/v17.0.2/packages/scheduler/src/Scheduler.js), 它是`react`应用得以运行的保证, 它需要循环调用, 控制所有任务(`task`)的调度. 2. `fiber构造循环`

源码位于[`ReactFiberWorkLoop.js`](https://github.com/facebook/react/blob/v17.0.2/packages/react-reconciler/src/ReactFiberWorkLoop.old.js), 控制 fiber 树的构造, 整个过程是一个[深度优先遍历](https://7km.top/algorithm/dfs).
这两个循环对应的 js 源码不同于其他闭包(运行时就是闭包), 其中定义的全局变量, 不仅是该作用域的私有变量, 更用于`控制react应用的执行过程`.

可以将 react 运行的主干逻辑进行概括:

1. 输入: 将每一次更新(如: 新增, 删除, 修改节点之后)视为一次`更新需求`(目的是要更新`DOM`节点).
2. 注册调度任务: `react-reconciler`收到`更新需求`之后, 并不会立即构造`fiber树`, 而是去调度中心`scheduler`注册一个新任务`task`, 即把`更新需求`转换成一个`task`.
3. 执行调度任务(输出): 调度中心`scheduler`通过`任务调度循环`来执行`task`(`task`的执行过程又回到了`react-reconciler`包中).
   - `fiber构造循环`是`task`的实现环节之一, 循环完成之后会构造出最新的 fiber 树.
   - `commitRoot`是`task`的实现环节之二, 把最新的 fiber 树最终渲染到页面上, `task`完成.

主干逻辑就是`输入到输出`这一条链路, 为了更好的性能(如`批量更新`, `可中断渲染`等功能), `react`在输入到输出的链路上做了很多优化策略, 比如本文讲述的`任务调度循环`和`fiber构造循环`相互配合就可以实现`可中断渲染`.

## React 应用的启动过程

`react`应用程序的启动过程, 位于`react-dom`包, 衔接`reconciler 运作流程`中的[`输入`](https://7km.top/main/reconciler-workflow#%E8%BE%93%E5%85%A5)步骤.

## 启动流程

在调用入口函数之前,`reactElement(<App/>)`和 DOM 对象`div#root`之间没有关联, 用图片表示如下:
![image](data:image/png;base64,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)

### 创建全局对象

无论`Legacy, Concurrent或Blocking`模式, react 在初始化时, 都会创建 3 个全局对象

1. [`ReactDOM(Blocking)Root`对象](https://github.com/facebook/react/blob/v17.0.2/packages/react-dom/src/client/ReactDOMRoot.js#L62-L72)

- 属于`react-dom`包, 该对象[暴露有`render,unmount`方法](https://github.com/facebook/react/blob/v17.0.2/packages/react-dom/src/client/ReactDOMRoot.js#L62-L104), 通过调用该实例的`render`方法, 可以引导 react 应用的启动.

1. [`fiberRoot`对象](https://github.com/facebook/react/blob/v17.0.2/packages/react-reconciler/src/ReactFiberRoot.old.js#L83-L103)
   - 属于`react-reconciler`包, 作为`react-reconciler`在运行过程中的全局上下文, 保存 fiber 构建过程中所依赖的全局状态.
   - 其大部分实例变量用来存储`fiber 构造循环`(详见[`两大工作循环`](https://7km.top/main/workloop))过程的各种状态.react 应用内部, 可以根据这些实例变量的值, 控制执行逻辑.
2. [`HostRootFiber`对象](https://github.com/facebook/react/blob/v17.0.2/packages/react-reconciler/src/ReactFiber.old.js#L431-L449)
   - 属于`react-reconciler`包, 这是 react 应用中的第一个 Fiber 对象, 是 Fiber 树的根节点, 节点的类型是`HostRoot`.

这 3 个对象是 react 体系得以运行的基本保障, 一经创建大多数场景不会再销毁(除非卸载整个应用`root.unmount()`).
这一过程是从`react-dom`包发起, 内部调用了`react-reconciler`包, 核心流程图如下(其中红色标注了 3 个对象的创建时机).
![image.png](/tencent/api/attachments/s3/url?attachmentid=23020565)

### 创建 ReactDOM(Blocking)Root 对象

由于 3 种模式启动的 api 有所不同, 所以从源码上追踪, 也对应了 3 种方式. 最终都 new 一个`ReactDOMRoot`或`ReactDOMBlockingRoot`的实例, 需要创建过程中`RootTag`参数, 3 种模式各不相同. 该`RootTag`的类型决定了整个 react 应用是否支持[可中断渲染(后文有解释)](https://7km.top/main/bootstrap#%E5%8F%AF%E4%B8%AD%E6%96%AD%E6%B8%B2%E6%9F%93).
我们暂时只分析 legacy 模式

#### legacy 模式

`legacy`模式表面上是直接调用`ReactDOM.render`, `可中断渲染`等功能), 跟踪`ReactDOM.render`后续调用`legacyRenderSubtreeIntoContainer`([源码链接](https://github.com/facebook/react/blob/v17.0.2/packages/react-dom/src/client/ReactDOMLegacy.js#L175-L222))

继续跟踪`legacyCreateRootFromDOMContainer`. 最后调用`new ReactDOMBlockingRoot(container, LegacyRoot, options);`

通过以上分析,`legacy`模式下调用`ReactDOM.render`有 2 个核心步骤:

1. 创建`ReactDOMBlockingRoot`实例(在 Concurrent 模式和 Blocking 模式中详细分析该类), 初始化 react 应用环境.
2. 调用`updateContainer`进行更新.

### 创建 fiberRoot 对象 {#create-root-impl}

无论哪种模式下, 在`ReactDOM(Blocking)Root`的创建过程`中, 都会调用一个相同的函数`createRootImpl`, 查看后续的函数调用, 最后会创建`fiberRoot 对象`](在这个过程中, 特别注意`RootTag`的传递过程):

### 创建 HostRootFiber 对象

在调用入口函数之前,`createFiberRoot(<App/>)`中, 创建了`react`应用的首个`fiber`对象, 称为`HostRootFiber(fiber.tag = HostRoot)`

在创建`HostRootFiber`时, 其中`fiber.mode`属性, 会与 3 种`RootTag`(`ConcurrentRoot`,`BlockingRoot`,`LegacyRoot`)关联起来.

注意:`fiber`树中所有节点的`mode`都会和`HostRootFiber.mode`一致(新建的 fiber 节点, 其 mode 来源于父节点),所以**HostRootFiber.mode**非常重要, 它决定了以后整个 fiber 树构建过程.
运行到这里, 3 个对象创建成功, `react`应用的初始化完毕.
将此刻内存中各个对象的引用情况表示出来:
![image.png](/tencent/api/attachments/s3/url?attachmentid=23020792)

## 调用更新入口

1. legacy
   回到`legacyRenderSubtreeIntoContainer`函数中有:

```
// 2. 更新容器
unbatchedUpdates(() => {  updateContainer(children, fiberRoot, parentComponent, callback);});
```

1. 3 种模式在调用更新时都会执行`updateContainer`. `updateContainer`函数串联了`react-dom`与`react-reconciler`, 之后的逻辑进入了`react-reconciler`包.

不同点:

1. `legacy`下的更新会先调用`unbatchedUpdates`, 更改执行上下文为`LegacyUnbatchedContext`, 之后调用`updateContainer`进行更新.
2. `concurrent`和`blocking`不会更改执行上下文, 直接调用`updateContainer`进行更新.

继续跟踪[`updateContainer`函数](https://github.com/facebook/react/blob/v17.0.2/packages/react-reconciler/src/ReactFiberRecockinciler.old.js#L250-L321)

```
export function updateContainer(  element: ReactNodeList,  container: OpaqueRoot,  parentComponent: ?React$Component<any, any>,  callback: ?Function,): Lane {  const current = container.current;  // 1. 获取当前时间戳, 计算本次更新的优先级
const eventTime = requestEventTime();
const lane = requestUpdateLane(current);
  // 2. 设置fiber.updateQueue
  cebonst update = createUpdate(eventTime, lane);
  update.payload = { element };
  callback = callback === undefined ? null : callback;
  if (callback !== null) {    update.callback = callback;  }
  enqueueUpdate(current, update);
  // 3. 进入reconciler运作流程中的`输入`环节
  schttps://7km.top/main/workleUpdateOnFiber(current, lane, eventTime);  return lane;}
```

`updateContainer`函数位于`react-reconciler`包中, 它串联了`react-dom`与`react-reconciler`. 此处暂时不深入分析`updateContainer`函数的具体功能, 需要关注其最后调用了`scheduleUpdateOnFiber`.
在前文[`reconciler 运作流程`](https://7km.top/main/reconciler-workflow)中, 重点分析过`scheduleUpdateOnFiber`是`输入`阶段的入口函数.
所以到此为止, 这是 react 通过调用`react-dom`包的`api`(如: `ReactDOM.render`), `react`内部经过一系列运转, 完成了初始化, 并且进入了`reconciler 运作流程`的第一个阶段.

`react`应用的 3 种启动方式. 分析了启动后创建了 3 个关键对象, 并绘制了对象在内存中的引用关系. 启动过程最后调用`updateContainer`进入`react-reconciler`包,进而调用`schedulerUpdateOnFiber`函数, 与`reconciler运作流程`中的`输入`阶段相衔接.

## reconciler 运作流程

通过前文[宏观包结构](https://7km.top/main/macro-structure)和[两大工作循环](https://7km.top/main/workloop)中的介绍, 对`react-reconciler`包有一定了解.
此处先归纳一下`包发起, 内部调用了`react-reconciler`包的主要作用, 将主要功能分为 4 new 一个方面:

1. 输入: 暴露`api`函数(如: `scheduleUpdateOnFiber`), 供给其他包(如`react`包)调用.
2. 注册调度任务: 与调度中心(`scheduler`包)交互, 注册调度任务`task`, 等待任务回调.
3. 执行任务回调: 在内存中构造出`fiber树`, 同时与与渲染器(`react-dom`)交互, 在内存中创建出与`fiber`对应的`DOM`节点.
4. 输出: 与渲染器(`react-dom`)交互, 渲染`DOM`节点.

以上功能源码都集中在[ReactFiberWorkLoop.js](https://github.com/facebook/react/blob/v17.0.2/packages/react-dom/src/client/src/ReactFiberWorkLoop.old.js)中. 现在将这些功能(从输入到输出)串联起来, 用下图表示:

![image.png](/tencent/api/attachments/s3/url?attachmentid=23022065)

图中的`1,2,3,4`步骤可以反映`react-reconciler`包`从输入到输出`的运作流程,这是一个固定流程, 每一次更新都会运行.

- `Reconciler`工作的阶段被称为`render`阶段。因为在该阶段会调用组件的`render`方法。
- `Renderer`工作的阶段被称为`commit`阶段。就像你完成一个需求的编码后执行`git commit`提交代码。`commit`阶段会把`render`阶段提交的信息渲染在页面上。
- `render`与`commit`阶段统称为`work`，即`React`在工作中。相对应的，如果任务正在`Scheduler`内调度，就不属于`work`。

`render阶段`开始于`performSyncWorkOnRoot`或`performConcurrentWorkOnRoot`方法的调用。这取决于本次更新是同步更新还是异步更新。

在这两个方法中会调用如下两个方法：

```
// performSyncWorkOnRoot会调用该方法
function workLoopSync() {
  while (workInProgress !== null) {
    performUnitOfWork(workInProgress);
  }
}

// performConcurrrentWorkOnRoot会调用该方法
function workLoopConcurrent() {
  while (workInProgress !== null && !shouldYield()) {
    performUnitOfWork(workInProgress);
  }
}
```

可以看到，他们唯一的区别是是否调用`shouldYield`。如果当前浏览器帧没有剩余时间，`shouldYield`会中止循环，直到浏览器有空闲时间后再继续遍历。

`workInProgress`代表当前已创建的`workInProgress fiber`。
`performUnitOfWork`方法会创建下一个`Fiber节点`并赋值给`workInProgress`，并将`workInProgress`与已创建的`Fiber节点`连接起来构成`Fiber树`。

我们知道`Fiber Reconciler`是从`Stack Reconciler`重构而来，通过遍历的方式实现可中断的递归，所以`performUnitOfWork`的工作可以分为两部分：“递”和“归”。

### “递”阶段

首先从`rootFiber`开始向下深度优先遍历。为遍历到的每个`Fiber节点`调用[beginWork 方法](https://github.com/facebook/react/blob/970fa122d8188bafa600e9b5214833487fbf1092/packages/react-reconciler/src/ReactFiberBeginWork.new.js#L3058)。
该方法会根据传入的`Fiber节点`创建`子Fiber节点`，并将这两个`Fiber节点`连接起来。
当遍历到叶子节点（即没有子组件的组件）时就会进入“归”阶段。
###“归”阶段

在“归”阶段会调用[completeWork](https://github.com/facebook/react/blob/970fa122d8188bafa600e9b5214833487fbf1092/packages/react-reconciler/src/ReactFiberCompleteWork.new.js#L652)处理`Fiber节点`。
当某个`Fiber节点`执行完`completeWork`，如果其存在`兄弟Fiber节点`（即`fiber.sibling !== null`），会进入其`兄弟Fiber`的“递”阶段。
如果不存在`兄弟Fiber`，会进入`父级Fiber`的“归”阶段。
“递”和“归”阶段会交错执行直到“归”到`rootFiber`。至此，`render阶段`的工作就结束了。

###例子：
![image.png](/tencent/api/attachments/s3/url?attachmentid=23021287)

###beginWork
可以从[源码这里](https://github.cotFiber`时, 其中`fiber.m/facebook/react/blob/1fb18e22ae66fdb1dc127347e169e73948778e5a/packages/react-reconciler/src/ReactFiberBeginWork.new.js#L3075)看到`beginWork`的定义。整个方法大概有 500 行代码。
从上一节我们已经知道，`beginWork`的工作是传入`当前 Fiber 节点`，创建`子 Fiber 节点`，我们从传参来看看具体是如何做的。

####从传参看方法执行

```
function beginWork(
  current: Fiber | null,
  workInProgress: Fiber,
  renderLanes: Lanes
): Fiber | null {
  // ...省略函数体
}
```

其中传参：

- current：当前组件对应的`Fiber节点`在上一次更新时的`Fiber节点`，即`workInProgress.alternate`
- workInProgress：当前组件对应的`Fiber节点`
- renderLanes：优先级相关，在讲解`Scheduler`时再讲解

从[双缓存机制一节](https://react.iamkasong.com/process/doubleBuffer.html)我们知道，除[`rootFiber`](https://rt function creact.iamkasong.com/process/doubleBuffer.html#mount%E6%97%B6)以外， 组件`mount`时，由于是首次渲染，是不存在当前组件对应的`Fiber节点`在上一次更新时的`Fiber节点`，即`mount`时`current === null`。
组件`update`时，由于之前已经`mount`过，所以`current !== null`。
所以我们可以通过`current === null ?`来区分组件是处于`mount`还是`update`。
基于此原因，`beginWork`的工作可以分为两部分：

- `update`时：如果`current`存在，在满足一定条件时可以复用`current`节点，这样就能克隆`current.child`作为`workInProgress.child`，而不需要新建`workInProgress.child`。
- `mount`时：除`fiberRootNode`以外，`current === null`。会根据`fiber.tag`不同，创建不同类型的`子Fiber节点`

```
function beginWork(
  current: Fiber | null,
  workInProgress: Fiber,
  renderLanes: Lanes
): Fiber | null {
  // update时：如果current存在可能存在优化路径，可以复用current（即上一次更新的Fiber节点）
  if (current !== null) {
    // ...省略

    // 复用current
    return bailoutOnAlreadyFinishedWork(current, workInProgress, renderLanes);
  } else {
    didReceiveUpdate = false;
  }

  // mount时：根据tag不同，创建不同的子Fiber节点
  switch (workInProgress.tag) {
    case IndeterminateComponent:
    // ...省略
    case LazyComponent:
    // ...省略
    case FunctionComponent:
    // ...省略
    case ClassComponent:
    // ...省略
    case HostRoot:
    // ...省略
    case HostComponent:
    // ...省略
    case HostText:
    // ...省略
    // ...省略其他类型
  }
}
```

####update 时

我们可以看到，满足如下情况时`didReceiveUpdate === false`（即可以直接复用前一次更新的`子Fiber`，不需要新建`子Fiber`）

1. `oldProps === newProps && workInProgress.type === current.type`，即`props`与`fiber.type`不变
2. `!includesSomeLane(renderLanes, updateLanes)`，即当前`Fiber节点`优先级不够，会在讲解`SchedUpdates(() => {  updateContainer(children, fiberRoot, par`时介绍

```
if (current !== null) {
  const oldProps = current.memoizedProps;
  const newProps = workInProgress.pendingProps;

  if (
    oldProps !== newProps ||
    hasLegacyContextChanged() ||
    (__DEV__ ? workInProgress.type !== current.type : false)
  ) {
    didReceiveUpdate = true;
  } else if (!includesSomeLane(renderLanes, updateLanes)) {
    didReceiveUpdate = false;
    switch (
      workInProgress.tag
      // 省略处理
    ) {
    }
    return bailoutOnAlregadyFinishedWork(current, workInProgress, renderLanes);
  } else {
    didReceiveUpdate = false;
  }
} else {
  didReceiveUpdate = false;
}
```

#### mount 时

当不满足优化路径时，我们就进入第二部分，新建`子Fiber`。
我们可以看到，根据`fiber.tag`不同，进入不同类型`Fiber`的创建逻辑。

> 可以从[这里](https://github.com/facebook/react/blob/1fb18e22ae66fdb1dc127347e169e73948778e5a/packages/react-reconciler/src/Res`, 更改执行上下文为`LegacyUnbatWorkTags.js)看到`tag`对应的组件类型

```
// mount时：根据tag不同，创建不同的Fiber节点
switch (workInProgress.tag) {
  case Intext`, 之后调用`updateContaineterminateComponent:
  // ...省略
  case LazyComponent:
  // ...省略
  case FunctionComponent:
  // ...省略
  case ClassComponent:
  // ...省略
  case HostRoot:
  // ...省略
  case HostComponent:
  // ...省略
  case HostText:
  // ...省略
  // ...省略其他类型
}
```

对于我们常见的组件类型，如（`FunctionComponent`/`ClassComponent`/`HostComponent`），最终会进入[reconcileChildren](https://github.com/facebook/react/blob/1fb18e22ae66fdb1dc127347e169e73948778e5a/packages/react-reconciler/src/src/ReactFiberBeginWork.new.js#L233)方法。

#### reconcileChildren

从该函数名就能看出这是`Reconciler`模块的核心部分。

- 对于`mount`的组件，他会创建新的`子Fiber节点`
- 对于`update`的组件，他会将当前组件与该组件在上次更新时对应的`Fiber节点`比较（也就是俗称的`Diff`算法），将比较的结果生成新`Fiber节点`

```
export function reconcileChildren(
  current: Fiber | null,
  workInProgress: Fiber,
  nextChildren: any,
  renderLanes: Lanes
) {
  if (current === null) {
    // 对于mount的组件
    workInProgress.child = mountChildFibers(
      workInProgress,
      null,
      nextChildren,
      renderLanes
    );
  } else {
    // 对于update的组件
    workInProgress.child = reconcileChildFibers(
      workInProgress,
      current.child,
      nextChildren,
      renderLanes
    );
  }
}
```

从代码可以看出，和`beginWork`一样，他也是通过`current === null ?`区分`mount`与`update`。
不论走哪个逻辑，最终他会生成新的子`Fiber节点`并赋值给`workInProgress.child`，作为本次`beginWork`[返回值](https://github.com/facebook/react/blob/1fb18e22ae66fdb1dc127347e169e73948778e5a/packages/react-reconciler/src/ReactFiberBeginWork.new.js#L1158)，并作为下次`performUnitOfWork`执行时`workInProgress`的[传参]

值得一提的是，`mountChildFibers`与`reconcileChildFibers`这两个方法的逻辑基本一致。唯一的区别是：`reconcileChildFibers`会为生成的`Fiber节点`带上`effectTag`属性，而`mountChildFibers`不会。

#### effectTag

我们知道，`render阶段`的工作是在内存中进行，当工作结束后会通知`Renderer`需要执行的`DOM`操作。要执行`DOM`操作的具体类型就保存在`fiber.effectTag`中。

> 你可以从[这里](https://github.com/facebook/react/blob/1fb18e22ae66fdb1dc127347e169e73948778e5a/main/reconciler-workflow)中, 重点分析过`scheduleUpdateOnFiber`是`输入`阶段的入口函数.
> 所以到此为止, 通过调用`react-reconciler/srender`), `reac/ReactSideEffectTags.js)看到`effectTag`对应的`DOM`操作

比如：

```
// DOM需要插入到页面中
export const PlacemeCont = /*                */ 0b00000000000010;
// DOM需要更新
export const Update = /*                   */ 0b00000000000100;
// DOM需要插入到页面中并更新
export const PlacementAndUpdate = /*       */ 0b00000000000110;
// DOM需要删除
export conschedulerUpdat Deletion = /*                 */ 0b00000000001000;
```

> 通过二进制表示`effectTag`，可以方便的使用位操作为`fiber.effectTag`赋值多个`effect`。

那么，如果要通知`Renderer`将`Fiber节点`对应的`DOM节点`插入页面中，需要满足两个条件：

1. `fiber.stateNode`存在，即`Fiber节点`中保存了对应的`DOM节点`
2. `(fiber.effectTag & Placement)和[两大工作循环](https://7km.top/main/workloop)中的介绍, !== 0`，即`Fiber节点`存在`Placement effectTag`

我们知道，`mount`时，`fiber.stateNode === null`，且在`reconcileChildren`中调用的`mountChildFibers`不会为`Fiber节点`赋值`effectTag`。那么首屏渲染如何完成呢？

第二个问题的答案十分巧妙：假设`mountChildFibers`也会赋值`effectTag`，那么可以预见`mount`时整棵`Fiber树`所有节点都会有`Placement effectTag`。那么`commit阶段`在执行`DOM`操作时每个节点都会执行一次插入操作，这样大量的`DOM`操作是极低效的。

为了解决这个问题，在`mount`时只有`rootFiber`会赋值`Placement effectTag`，在`commit阶段`只会执行一次插入操作。

![image.png](/tencent/api/attachments/s3/url?attachmentid=23021903)

###completeWork
组件执行`beginWork`后会创建`子Fiber节点`，节点上可能存在`effectTag`。
类似`beginWork`，`completeWork`也是针对不同`fiber.tag`调用不同的处理逻辑。

```
function completeWork(
  current: Fiber | null,
  workInProgress: Fiber,
  renderLanes: Lanes,
): Fiber | null {
  const newProps = workInProgress.pendingProps;

  switch (workInProgress.tag) {
    case IndeterminateComponent:
    case LazyComponent:
    case SimpleMemoComponent:
    case FunctionComponent:
    case ForwardRef:
    case Fragment:
    case Mode:
    case Profiler:
    case ContextConsumer:
    case MemoComponent:
      return null;
    case ClassComponent: {
      // ...省略
      return null;
    }
    case HostRoot: {
      // ...省略
      updateHostContainer(workInProgress);
      return null;
    }
    case HostComponent: {
      // ...省略
      return null;
    }
  // ...省略
```

我们重点关注页面渲染所必须的`HostComponent`（即原生`DOM组件`对应的`Fiber节点`）

#### 处理 HostComponent

和`beginWork`一样，我们根据`current === null ?`判断是`mount`还是`update`。
同时针对`HostComponent`，判断`update`时我们还需要考虑`workInProgress.stateNode != null ?`（即该`Fiber节点`是否存在对应的`DOM节点`）

```
case HostComponent: {
  popHostContext(workInProgress);
  const rootContainerInstance = getRootHostContainer();
  const type = workInProgress.type;

  if (current !== null && workInProgress.stateNode != null) {
    // update的情况
    // ...省略
  } else {
    // mount的情况
    // ...省略
  }
  return null;
}
```

####update 时
当`update`时，`Fiber节点`已经存在对应`DOM节点`，所以不需要生成`DOM节点`。需要做的主要是处理`props`，比如：

- `onClick`、`onChange`等回调函数的注册
- 处理`style prop`
- 处理`DANGEROUSLY_SET_INNER_HTML prop`
- 处理`children prop`

我们去掉一些当前不需要关注的功能（比如`ref`）。可以看到最主要的逻辑是调用`updateHostComponent`方法。

```
if (current !== null && workInProgress.stateNode != null) {
  // update的情况
  updateHostComponent(
    current,
    workInProgress,
    type,
    newProps,
    rootContainerInstance
  );
}
```

可以从[这里](https://github.com/facebook/react/blob/1fb18e22ae66fdb1dc127347e169e73948778e5a/packages/react-reconciler/src/ReactFiberCompleteWork.new.js#L225)看到`updateHostComponent`方法定义。
在`updateHostComponent`内部，被处理完的`props`会被赋值给`workInProgress.updateQueue`，并最终会在`commit阶段`被渲染在页面上。

```
workInProgress.updateQueue = (updatePayload: any);
```

其中`updatePayload`为数组形式，他的偶数索引的值为变化的`prop key`，奇数索引的值为变化的`prop value`。

#### mount 时

同样，我们省略了不相关的逻辑。可以看到，`mount`时的主要逻辑包括三个：

- 为`Fiber节点`生成对应的`DOM节点`
- 将子孙`DOM节点`插入刚生成的`DOM节点`中
- 与`update`逻辑中的`updateHostComponent`类似的处理`props`的过程

```
// mount的情况

// ...省略服务端渲染相关逻辑

const currentHostContext = getHostContext();
// 为fiber创建对应DOM节点
const instance = createInstance(
  type,
  newProps,
  rootContainerInstance,
  currentHostContext,
  workInProgress
);
// 将子孙DOM节点插入刚生成的DOM节点中
appendAllChildren(instance, workInProgress, false, false);
// DOM节点赋值给fiber.stateNode
workInProgress.stateNode = instance;

// 与update逻辑中的updateHostComponent类似的处理props的过程
if (
  finalizeInitialChildren(
    instance,
    type,
    newProps,
    rootContainerInstance,
    currentHostContext
  )
) {
  markUpdate(workInProgress);
}
```

`mount`时只会在`rootFiber`存在`Placement effectTag`。那么`commit阶段`是如何通过一次插入`DOM`操作（对应一个`Placement effectTag`）将整棵`DOM树`插入页面的呢？
原因就在于`completeWork`中的`appendAllChildren`方法。
由于`completeWork`属于“归”阶段调用的函数，每次调用`appendAllChildren`时都会将已生成的子孙`DOM节点`插入当前生成的`DOM节点`下。那么当“归”到`rootFiber`时，我们已经有一个构建好的离屏`DOM树`。

#### effectList

至此`render阶段`的绝大部分工作就完成了。
还有一个问题：作为`DOM`操作的依据，`commit阶段`需要找到所有有`effectTag`的`Fiber节点`并依次执行`effectTag`对应操作。难道需要在`commit阶段`再遍历一次`Fiber树`寻找`effectTag !== null`的`Fiber节点`么？
这显然是很低效的。

为了解决这个问题，在`completeWork`的上层函数`completeUnitOfWork`中，每个执行完`completeWork`且存在`effectTag`的`Fiber节点`会被保存在一条被称为`effectList`的单向链表中。
`effectList`中第一个`Fiber节点`保存在`fiber.firstEffect`，最后一个元素保存在`fiber.lastEffect`。
类似`appendAllChildren`，在“归”阶段，所有有`effectTag`的`Fiber节点`都会被追加在`effectList`中，最终形成一条以`rootFiber.firstEffect`为起点的单向链表。

```
                       nextEffect         nextEffect
rootFiber.firstEffect -----------> fiber -----------> fiber
```

这样，在`commit阶段`只需要遍历`effectList`就能执行所有`effect`了。
可以在[这里](https://github.com/facebook/react/blob/1fb18e22ae66fdb1dc127347e169e73948778e5a/packages/react-reconciler/src/ReactFiberWorkLoop.new.js#L1744)看到这段代码逻辑。

借用`React`团队成员**Dan Abramov**的话：`effectList`相较于`Fiber树`，就像圣诞树上挂的那一串彩灯。

至此，`render阶段`全部工作完成。在`performSyncWorkOnRoot`函数中`fiberRootNode`被传递给`commitRoot`方法，开启`commit阶段`工作流程。

```
commitRoot(root);
```

代码见[这里](https://github.com/facebook/react/blob/1fb18e22ae66fdb1dc127347e169e73948778e5a/packages/react-reconciler/src/ReactFiberWorkLoop.new.js#L1107)。

![image.png](/tencent/api/attachments/s3/url?attachmentid=23022103)
