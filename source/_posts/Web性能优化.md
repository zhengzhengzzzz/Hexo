# 一、发送请求时

## 1.避免多余重定向

重定向分为 301 的永久重定向和 302 的临时重定向。建议贴合语义，例如服务迁移的情况下，使用 301 重定向。对 SEO 也会更友好。并且每次重定向都是有请求耗时的，**所以建议避免过多的重定向**。

## 2.Resource Hints

### 2.1 预解析

DNS 解析流程可能会很长，很耗时，所以整个 DNS 服务，包括客户端都会有缓存机制，这个作为前端不好涉入，但在 DNS 解析上，前端可以通过其他手段来加速。
第一步：首先打开 DNS 预解析

```TS
<meta http-equiv="x-dns-prefetch-control" content="on">
```

第二步：手动添加解析

```
<link rel="dns-prefetch" href="//www.img.com">
```

这样之后用户的某个动作，触发了 [www.img.com](http://www.img.com/) 域名下的远程请求时，就避免了 DNS 解析的步骤。
**使用场景：**

1. 静态资源域名（如 CDN）
2. 未来即将会发生跳转的域名
3. 会重定向的域名
   **注意事项：**
4. 浏览器并[不保证](https://www.w3.org/TR/resource-hints/#dns-prefetch)一定会去解析域名，可能会根据当前的网络、负载等状况做决定
5. Chrome 会使用了 8 个异步线程来处理 DNS 预解析，所以过多的 prefetch 并不一定能提高网页加载效率

### 2.2 预连接

preconnect 会触发 DNS 预解析
建立连接需要 TCP 协议握手，有些还会有 TLS/SSL 协议，这些都会导致连接的耗时。使用 [Preconnect](https://www.w3.org/TR/resource-hints/#preconnect) 可以帮助你告诉浏览器：“我有一些资源会用到某个源（origin），你可以帮我预先建立连接。

```
<link rel="preconnect" href="//example.com"> <link rel="preconnect" href="//cdn.example.com" crossorigin>
```

### 2.3 预加载

#### 2.3.1 Prefetch

可以用 Prefetch 来指定在紧接着之后的操作或浏览中需要使用到的资源，让浏览器在空闲时提前获取。由于仅仅是提前获取资源，因此浏览器不会对资源进行预处理，实际使用中效果也很好。 示例：
第一步：在使用 React 做懒加载时，可借助 Webpack 指定 Prefetch

```
const Game = lazy(() => import(/* webpackPrefetch: true */ './game'));
```

第二步：此时你会发现对应的 Chunk 被加上了 Prefetch 标识

```
 <link rel="prefetch" as="script" href="/static/js/6.f0a869f6.chunk.js">
```

第三步：此时访问到对应页面时会发现响应头状态码带上了缓存标识

```
Request Method: GET
Status Code: 200 OK (from **prefetch cache**)
```

#### 2.3.2 Preload

错误的设置会导致阻塞页面加载，通常可以对字体资源设置 preload，来避免无样式文本 (FOUT) 闪烁的情况

在遇到需要 Preload 的资源时，浏览器会 立刻 进行预获取，并将结果放在内存中，资源的获取不会影响页面 parse 与 load 事件的触发。

```
<link rel="preload" href="./nextpage.js" as="script">
```

#### 2.3.3 Prerender

如果你指定 Prerender 一个页面，那么它依赖的其他资源，像 <script>、 等页面所需资源也可能会被下载与处理。但是预处理会基于当前机器、网络情况的不同而被不同程度地推迟。例如，会根据 CPU、GPU 和内存的使用情况，以及请求操作的幂等性而选择不同的策略或阻止该操作。

```
<link rel="prerender" href="//sample.com/nextpage.html">
```

# 二、运行时

## 1. 滚动事件优化

当一个事件频繁触发，而你希望间隔一定的时间再触发相应的函数时就会使用节流（**throttle**）。例如在页面滚动时，每 200ms 进行一次页面背景颜色的修改。 当一个事件频繁触发，而你希望在事件触发结束一段时间后（此段时间内不再有触发）才实际触发响应函数时会使用防抖（**debounce**）。例如用户一直点击按钮，但你不希望频繁发送请求，你就可以设置当点击后 200ms 内用户不再点击时才发送请求。 使用 [scroll-behavior](https://developer.mozilla.org/zh-CN/docs/Web/CSS/scroll-behavior) 优化你的滚动体验，体验优化十分明显，往往使用在返回顶部等功能上

## 2. 长列表优化

通常有三种方式：

- **分页加载**：解决了数据过多问题，通过数据分页的方式减少了首次页面加载的数据和 DOM 数量。
- 分片加载：优先显示页面数据在加载其他数据。会出现页面阻塞和性能问题。
- **虚拟列表**：只渲染可视区域的列表项，非可见区域的完全不渲染，在滚动条滚动时动态更新列表项，这里通常使用成熟的[第三方库](https://github.com/bvaughn/react-virtualized)去解决。

## 3. 避免 JS 运行时过长（Long Tasks）

建议参考腾讯文档的[实践](https://km.woa.com/group/46654/articles/show/524816?kmref=search&from_page=1&no=1) 把大任务拆成一个个持续时间更短的小任务,

```
const schduler = (tasks) => {
    const DEFAULT_RUNTIME = 16;
    const { port1, port2 } = new MessageChannel();
    let sum = 0;

    // 运行器
    const runner = () => {
        const prevTime = performance.now();
        do {
            if (tasks.length === 0) {
                return;
            }
            const task = tasks.shift();
            const value = task();
            sum += value;
        } while (performance.now() - prevTime < DEFAULT_RUNTIME);
        // 当前分片执行完成后开启下一个分片
        port2.postMessage('');
    };

    port1.onmessage = function () {
        runner();
    };

    port2.postMessage('');
};
```

## 4. 延迟执行

懒执行：当我需要某个值时，才去计算 延迟执行：利用 setTimeout、requestIdleCallback 这样的方法把计算放到后续的事件循环或空闲时刻。

## 5.并行计算

对于一些 CPU 密集型的计算场景，除了在主 JavaScript 线程中拆分调度任务、异步执行之外，我们还可以考虑将计算与主线程并行。在浏览器中启用并行线程可以放在 [Web Worker](https://www.html5rocks.com/en/tutorials/workers/basics/)^[8]^ 中。 最近发现一个很有趣的应用：[partytown](https://partytown.builder.io/)，在 web worker 中加载第三方资源。

## 6. 善于利用渲染合成层

### 6.1 什么是合成层 从接收到文件到内容展示大致经历了以下步骤：

![image.png](/tencent/api/attachments/s3/url?attachmentid=23037526)
在 Composite 阶段，渲染引擎会为特定的节点生成专用的图层，并生成一棵对应的图层树（LayerTree） ，我们通过 Chorme devtools Layers 查看页面中的图层信息。 ![image.png](/tencent/api/attachments/s3/url?attachmentid=23037558)但并不是布局树的每个节点都包含一个图层，如果一个节点没有对应的层，那么这个节点就从属于父节点的图层。

### 6.2 如何创建合成层 一般来说拥有层叠上下文属性的元素会被提升为单独的一层 ：

1. 文档根元素（<html>）；
2. position 值为 absolute relative fixed sticky 且 z-index 值不为 auto 的元素；
3. opacity 属性值小于 1 的元素；
4. transform/filter clip-path/mask mask-image / mask-border，属性不为 none 的元素；
5. will-change 值设定了任一属性；
6. contain 属性值为 layout、paint 或包含它们其中之一的合成值
7. 对 opacity， filter，transform，background-filter 应用动画
8. video, iframe, canvas ### 6.3 如何利用合成层
   合成层的好处：
9. 合成层的位图，**会交由 GPU 合成**，比 CPU 处理要快；
10. 当需要 repaint 时，只需要 repaint 本身，**不会影响到其他的层**
11. 对于 transform 和 opacity 效果，不会触发 layout 和 paint

#### 6.3.1 利用 will-change

表示该元素在未来会发生变化，元素将修改特定的属性，让浏览器事先进行必要的优化。 示例： **在父元素上使用 will-change**，子元素上使用动画：

```
.animate-element-parent {
    will-change: opacity;
}

.animate-element {
    transition: opacity .2s linear
}
```

值得注意的是，这个属性不能够滥用，参考[MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/will-change)给出的解释

1. 不要将 will-change 应用到太多元素上
2. 有节制地使用
3. 不要过早应用 will-change 优化
4. 给它足够的工作时间

#### 6.3.2 使用 transform 或者 opacity 来实现动画效果

根据[CSS Triggers](https://csstriggers.com/) 给出的信息：元素提升为合成层后，transform 和 opacity 不会触发 paint，如果不是合成层，则其依然会触发 paint

### 6.4 避免层爆炸

[CSS3 硬件加速也有坑](https://link.juejin.cn/?target=https%3A%2F%2Fdiv.io%2Ftopic%2F1348) 这篇文章提供了一个很有趣的 [DEMO](https://link.juejin.cn/?target=http%3A%2F%2Ffouber.github.io%2Ftest%2Flayer%2F)，这个 DEMO 页面中包含了一个 h1 标题，它对 transform 应用了 animation 动画，进而导致被放到了合成层中渲染。由于 animation transform 的特殊性（动态交叠不确定），隐式合成在不需要交叠的情况下也能发生，就导致了页面中所有 z-index 高于它的节点所对应的渲染层全部提升为合成层，导致出现了层爆炸。

## 7. 利用 CSS 属性

### 7.1 使用 contain: strict

### 7.1 使用 contain: strict

该属性允许我们指定特定的 DOM 元素和它的子元素，让它们能够独立于整个 DOM 树结构之外。它能够让浏览器有能力只对部分元素进行重绘、重排，而不必每次针对整个页面。 属性值：

1. layout ：该值表示元素的内部布局不受外部的任何影响，同时该元素以及其内容也不会影响以上级
2. paint ：该值表示元素的子级不能在该元素的范围外显示，该元素不会有任何内容溢出（或者即使溢出了，也不会被显示）
3. size ：该值表示元素盒子的大小是独立于其内容，也就是说在计算该元素盒子大小的时候是会忽略其子元素
4. content ：contain: layout paint 的简写
5. strict ：contain: layout paint size 的简写

有关于 contain 的更多内容参考 ：
[contain-property](https://www.w3.org/TR/css-contain-2/#contain-property) [Helping Browsers Optimize With The CSS Contain Property](https://www.smashingmagazine.com/2019/12/browsers-containment-css-contain-property/) [CSS contain Property](https://termvader.github.io/css-contain/)

### 7.2 使用 content-visibility

`content-visibility`允许我们推迟我们选择的 HTML 元素渲染。 一般很难清楚明白使用哪个 contain 属性，因为只有在指定了适当的值后，浏览器才开始优化。我们可以在一个元素上使用 content-visibility: auto 来直接的提升页面的渲染性能。 如果元素不在屏幕上，这不会渲染其后代。浏览器在不考虑元素任何内容的情况下确定元素的大小，在此处则跳过大多数渲染,当元素接近视口时，开始绘制元素的内容。这使得渲染工作能够及时被用户看到。 由于没有在可视区域的内容没有被渲染，所以滚动条的渲染会出现不准确及抖动的问题，可以使用 contain-intrinsic-size 来对内容做一个预估

```
.paragraph {
    content-visibility: auto;
    contain-intrinsic-size: 320px;
}
```

# 三、静态资源优化

## 1. 字体优化

当你使用外部字体时往往需要引入额外的字体包，可是这个字体包通常会很大，而页面中只可能需要 100 个字，所以要对字体进行体积优化。

### 1.1 字体体积优化

这里推荐使用 [font-spider-plus](https://github.com/allanguys/font-spider-plus) 完成

#### 1.1.1 如何使用

第一步：首先全局安装插件

```
npm i font-spider-plus -g
mkdir index
cd index
mkdir fonts
```

第二步：创建 html 文件，并写入配置，将字体源文件放入 fonts 文件夹下

```
<div>
  腾讯游戏流量伙伴计划
</div>
<style>
  @font-face {
    font-family: 'font';
    src:
      url('./fonts/TencentSans-W7.ttf') format('truetype');
      font-weight: normal;
      font-style: normal;
  }
  div  {
    font-family:font;
  }
</style>
```

第三步： 执行生成文件命令

```
fsp local index.html
```

#### 1.1.2 效果对比

|          | 优化前 | 优化后 |
| :------- | ------ | ------ |
| 文件体积 | 8.4M   | 4kb    |

### 1.2 字体加载时优化

在字体加载的期间，浏览器页面是默认不展示文本内容的。即我们常说的 FOIT (Flash of Invisible Text)。 在现代浏览器中，FOIT 持续至多 3 秒，影响 CLS 指标，带来糟糕的用户体验。所以我们要关注如何平滑的加载字体。

#### 1.2.1 font-display

他可以让 FOIT 的默认行为变为 FOUT (Flash of Unstyled Text)，即先会使用默认字体样式展示文本，字体加载完毕后再将文本的字体样式进行替换。

```
@font-face {
    font-family: 'TencentSansW7';
    src: url('~@/assets/font/TencentSans-W7.ttf');
    font-display: optional; // 等待一小段时间（100ms），如果字体加载完则使用该字体，否则是否默认字体，并不再替换，
}
```

#### 1.2.2 内联字体

可以将字体文件转为 base64 的字符串，设置到 @font-face 里的 src 属性上：

```
@font-face {
    font-family: 'TencentSansW7';
    src: url('data:application/x-font-woff;charset=utf-8;base64,d09G…') format('woff2');
}
```

## 2. JS 体积优化

### 2.1 优化 Babel 的使用

#### 2.1.1 @Babel/plugin-transform-runtime

示例： 在我们使用 babel 编译以下代码时

```
class Person{}
```

编译后：

```
function _instanceof(left, right) {
  if (right != null && typeof Symbol !== "undefined" &&   right[Symbol.hasInstance]) {
    return !!right[Symbol.hasInstance](left);
  }
  else {
    return left instanceof right;
  }
}
function _classCallCheck(instance, Constructor) {
  if (!_instanceof(instance, Constructor)) { throw new TypeError("Cannot call a class as a function"); }
}
var Person = function Person() {
  _classCallCheck(this, Person);
};
```

可以看到 \_instanceof 和\_classCallCheck 都是 Babel 内置的 helpers 函数。如果每个 class 编译结果都在代码中植入这些 helpers 具体内容，对产出代码体积就会有明显恶化影响。 启用插件：

```
{
  "plugins": [
    "@babel/plugin-transform-runtime",
  ]
}
```

这时使用启用@Babel/plugin-transform-runtime 插件后

```
var _interopRequireDefault = require("@babel/runtime/helpers/interopRequireDefault");
var _classCallCheck2 = _interopRequireDefault(require("@babel/runtime/helpers/classCallCheck"));
var Person = function Person() {
    (0, _classCallCheck2.default)(this, Person);
};
```

从上述代码我们可以看到，\_classCallCheck 作为模块依赖被引入文件中，基于打包工具的 cache 能力，从而减少了产出代码体积。 上述代码引用到的@babel/runtime 它含有 Babel 编译所需的一些运行时 helpers 函数，供业务代码引入模块化的 Babel helpers 函数，同时提供了 [regenerator-runtime](https://www.npmjs.com/package/regenerator-runtime)，对 generator 和 async 函数进行编译降级。

#### 2.1.2 按需引入 Babel-polyfill

但是很多场景下我们可能只是使用了少量需要 polyfill 的 api，这个时候需要去做按需加载

```
{
  "presets": [
    [
      "@babel/preset-env",
      {
        "useBuiltIns": "usage",
      },
    ]
  ],
}
```

## 2.2 代码分割

**请求数和大小之间的权衡。HTTP2 普及后倾向于多文件，单个文件更小，提高缓存命中率和总体资源大小。**

- **将依赖按照 package name + version 拆分**，对于中小型项目来说，一个页面不会有太多依赖

```
cacheGroups: { vendors:
	{ test: /[\\/]node_odules[\\/]/, priority: 10, chunks: 'async', name(module: any) { // e.g. node_modules/.pnpm/lodash-es@4.17.21/node_modules/lodash-es const path = module.context.replace(/.pnpm[\\/]/, ''); const packageName = path.match( /[\\/]node_modules[\\/](.*?)([\\/]|$)/, )[1]; return `npm.${packageName .replace(/@/g, '_at_') .replace(/\+/g, '_')}`; }, }, },
```

- **将依赖按照基础库 + 被引用次数**，对于大型项目来说，不会出现过多的 HTTP 请求，也比较好的利用了缓存
