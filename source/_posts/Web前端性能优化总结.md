---
title: Web前端性能优化
date: 2024-7-20 16:34:27
tags: 前端 git
categories: bo‘k
---

从输入一个 URL 到页面展示的过程，将性能优化为发送请求时，运行时优化，静态资源优化，缓存四个阶段。
一、指标与度量
1.1 LCP
用于衡量标准报告视口内可见的最大内容元素的渲染时间。为了提供良好的用户体验，网站应努力在开始加载页面的前 2.5 秒内进行 最大内容渲染。
● Largest contentful paint (LCP ： 测量页面开始加载到最大文本块内容或图片显示在页面中的时间。
1.2 FID
记录用户和页面进行首次交互操作所花费的时间 。FID 指标影响用户对页面交互性和响应性的第一印象。 为了提供良好的用户体验，站点应努力使首次输入延迟小于 100 毫秒。
● First input delay (FID) ：测量用户首次与网站进行交互(例如点击一个链接、按钮)到浏览器真正进行响应的时间。
1.3 CLS
CLS 会测量在页面的整个生命周期中发生的每个意外的样式移动的所有单独布局更改得分的总和。布局的移动可能发生在可见元素从一帧到下一帧改变位置的任何时候。为了提供良好的用户体验，网站应努力使 CLS 分数小于 0.1 。
● Cumulative layout shift (CLS)： 测量从页面开始加载到状态变为隐藏过程中，发生不可预期的 layout shifts 的累积分数。
PageSpeed Insights 可以衡量一个网页的性能
在 JavaScript 中衡量 LCP
如需在 JavaScript 中衡量 LCP，您可以使用 Largest Contentful Paint API。以下示例展示了如何创建 PerformanceObserver 来监听 largest-contentful-paint 条目并将其记录到控制台中。
new PerformanceObserver((entryList) => {
for (const entry of entryList.getEntries()) {
console.log('LCP candidate:', entry.startTime, entry);
}
}).observe({type: 'largest-contentful-paint', buffered: true});
二、发送请求时

1. 避免多余重定向
   重定向分为 301 的永久重定向和 302 的临时重定向。建议贴合语义，例如服务迁移的情况下，使用 301 重定向。对 SEO 也会更友好。并且每次重定向都是有请求耗时的，所以建议避免过多的重定向。
2. Resource Hints
2.1 预解析
DNS 解析流程可能会很长，很耗时，所以整个 DNS 服务，包括客户端都会有缓存机制，这个作为前端不好涉入，但在 DNS 解析上，前端可以通过其他手段来加速。
第一步：首先打开 DNS 预解析
<meta http-equiv="x-dns-prefetch-control" content="on">
第二步：手动添加解析
<link rel="dns-prefetch" href="//www.img.com">
这样之后用户的某个动作，触发了 www.img.com 域名下的远程请求时，就避免了 DNS 解析的步骤。
使用场景：
3. 静态资源域名（如 CDN）
4. 未来即将会发生跳转的域名
5. 会重定向的域名
   注意事项：
6. 浏览器并不保证一定会去解析域名，可能会根据当前的网络、负载等状况做决定
7. Chrome 会使用了 8 个异步线程来处理 DNS 预解析，所以过多的 prefetch 并不一定能提高网页加载效率
2.2 预连接
preconnect 会触发 DNS 预解析
建立连接需要 TCP 协议握手，有些还会有 TLS/SSL 协议，这些都会导致连接的耗时。使用 Preconnect 可以帮助你告诉浏览器：“我有一些资源会用到某个源（origin），你可以帮我预先建立连接。
<link rel="preconnect" href="//example.com">
<link rel="preconnect" href="//cdn.example.com" crossorigin>
2.3 预加载
2.3.1 Prefetch
可以用 Prefetch 来指定在紧接着之后的操作或浏览中需要使用到的资源，让浏览器在空闲时提前获取。由于仅仅是提前获取资源，因此浏览器不会对资源进行预处理，实际使用中效果也很好。 示例：
可以用 Prefetch 来指定在紧接着之后的操作或浏览中需要使用到的资源，让浏览器在空闲时提前获取。由于仅仅是提前获取资源，因此浏览器不会对资源进行预处理，实际使用中效果也很好。 示例：
第一步：在使用React做懒加载时，可借助Webpack指定Prefetch
const Game = lazy(() => import(/* webpackPrefetch: true */ './game'));
第二步：此时你会发现对应的Chunk被加上了Prefetch标识
<link rel="prefetch" as="script" href="/static/js/6.f0a869f6.chunk.js">
第三步：此时访问到对应页面时会发现响应头状态码带上了缓存标识
Request Method: GET
Status Code: 200 OK (from **prefetch cache**)
2.3.2 Preload
错误的设置会导致阻塞页面加载，通常可以对字体资源设置preload，来避免无样式文本 (FOUT) 闪烁的情况
在遇到需要 Preload 的资源时，浏览器会 立刻 进行预获取，并将结果放在内存中，资源的获取不会影响页面 parse 与 load 事件的触发。
<link rel="preload" href="./nextpage.js" as="script">

2.3.3 Prerender
如果你指定 Prerender 一个页面，那么它依赖的其他资源，像 <script>、 等页面所需资源也可能会被下载与处理。但是预处理会基于当前机器、网络情况的不同而被不同程度地推迟。例如，会根据 CPU、GPU 和内存的使用情况，以及请求操作的幂等性而选择不同的策略或阻止该操作。

<link rel="prerender" href="//sample.com/nextpage.html">
三、运行时
1. 滚动事件优化
当一个事件频繁触发，而你希望间隔一定的时间再触发相应的函数时就会使用节流（throttle）。例如在页面滚动时，每 200ms 进行一次页面背景颜色的修改。 当一个事件频繁触发，而你希望在事件触发结束一段时间后（此段时间内不再有触发）才实际触发响应函数时会使用防抖（debounce）。例如用户一直点击按钮，但你不希望频繁发送请求，你就可以设置当点击后 200ms 内用户不再点击时才发送请求。 使用 scroll-behavior 优化你的滚动体验，体验优化十分明显，往往使用在返回顶部等功能上
2. 长列表优化
通常有三种方式：
● 分页加载：解决了数据过多问题，通过数据分页的方式减少了首次页面加载的数据和DOM数量。
● 分片加载：优先显示页面数据在加载其他数据。会出现页面阻塞和性能问题。
● 虚拟列表：只渲染可视区域的列表项，非可见区域的完全不渲染，在滚动条滚动时动态更新列表项，这里通常使用成熟的第三方库去解决。
3. 避免JS运行时过长（Long Tasks）
建议参考腾讯文档的实践 把大任务拆成一个个持续时间更短的小任务,
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

4. 延迟执行
   懒执行：当我需要某个值时，才去计算 延迟执行：利用 setTimeout、requestIdleCallback 这样的方法把计算放到后续的事件循环或空闲时刻。 5.并行计算
   对于一些 CPU 密集型的计算场景，除了在主 JavaScript 线程中拆分调度任务、异步执行之外，我们还可以考虑将计算与主线程并行。在浏览器中启用并行线程可以放在 Web Worker[8] 中。 最近发现一个很有趣的应用：partytown，在 web worker 中加载第三方资源。
5. 善于利用渲染合成层
   6.1 什么是合成层
   从接收到文件到内容展示大致经历了以下步骤：
   JS Style Layout Paint Composite
   在 Composite 阶段，渲染引擎会为特定的节点生成专用的图层，并生成一棵对应的图层树（LayerTree） ，我们通过 Chorme devtools Layers 查看页面中的图层信息。但并不是布局树的每个节点都包含一个图层，如果一个节点没有对应的层，那么这个节点就从属于父节点的图层。
   6.2 如何创建合成层
   一般来说拥有层叠上下文属性的元素会被提升为单独的一层 ：
6. 文档根元素（<html>）；
7. position 值为 absolute relative fixed sticky 且 z-index 值不为 auto 的元素；
8. opacity 属性值小于 1 的元素；
9. transform/filter clip-path/mask mask-image / mask-border，属性不为 none 的元素；
10. will-change 值设定了任一属性；
11. contain 属性值为 layout、paint 或包含它们其中之一的合成值
12. 对 opacity， filter，transform，background-filter 应用动画
13. video, iframe, canvas
    6.3 如何利用合成层
    合成层的好处：
14. 合成层的位图，会交由 GPU 合成，比 CPU 处理要快；
15. 当需要 repaint 时，只需要 repaint 本身，不会影响到其他的层
16. 对于 transform 和 opacity 效果，不会触发 layout 和 paint
    6.3.1 利用 will-change
    表示该元素在未来会发生变化，元素将修改特定的属性，让浏览器事先进行必要的优化。 示例： 在父元素上使用 will-change，子元素上使用动画：

.animate-element-parent {
will-change: opacity;
}

.animate-element {
transition: opacity .2s linear
}
值得注意的是，这个属性不能够滥用，参考 MDN 给出的解释

1. 不要将 will-change 应用到太多元素上
2. 有节制地使用
3. 不要过早应用 will-change 优化
4. 给它足够的工作时间
   6.3.2 使用 transform 或者 opacity 来实现动画效果
   根据 CSS Triggers 给出的信息：元素提升为合成层后，transform 和 opacity 不会触发 paint，如果不是合成层，则其依然会触发 paint
   CSS3 硬件加速也有坑 这篇文章提供了一个很有趣的 DEMO，这个 DEMO 页面中包含了一个 h1 标题，它对 transform 应用了 animation 动画，进而导致被放到了合成层中渲染。由于 animation transform 的特殊性（动态交叠不确定），隐式合成在不需要交叠的情况下也能发生，就导致了页面中所有 z-index 高于它的节点所对应的渲染层全部提升为合成层，导致出现了层爆炸。
5. 利用 CSS 属性
   7.1 使用 contain: strict
   该属性允许我们指定特定的 DOM 元素和它的子元素，让它们能够独立于整个 DOM 树结构之外。它能够让浏览器有能力只对部分元素进行重绘、重排，而不必每次针对整个页面。 属性值：
6. layout ：该值表示元素的内部布局不受外部的任何影响，同时该元素以及其内容也不会影响以上级
7. paint ：该值表示元素的子级不能在该元素的范围外显示，该元素不会有任何内容溢出（或者即使溢出了，也不会被显示）
8. size ：该值表示元素盒子的大小是独立于其内容，也就是说在计算该元素盒子大小的时候是会忽略其子元素
9. content ：contain: layout paint 的简写
10. strict ：contain: layout paint size 的简写
    有关于 contain 的更多内容参考 ：
    contain-property Helping Browsers Optimize With The CSS Contain Property CSS contain Property
    7.2 使用 content-visibility
    content-visibility 允许我们推迟我们选择的 HTML 元素渲染。 一般很难清楚明白使用哪个 contain 属性，因为只有在指定了适当的值后，浏览器才开始优化。我们可以在一个元素上使用 content-visibility: auto 来直接的提升页面的渲染性能。 如果元素不在屏幕上，这不会渲染其后代。浏览器在不考虑元素任何内容的情况下确定元素的大小，在此处则跳过大多数渲染,当元素接近视口时，开始绘制元素的内容。这使得渲染工作能够及时被用户看到。 由于没有在可视区域的内容没有被渲染，所以滚动条的渲染会出现不准确及抖动的问题，可以使用 contain-intrinsic-size 来对内容做一个预估
    .paragraph {
    content-visibility: auto;
    contain-intrinsic-size: 320px;
    }
    五、静态资源优化
11. 字体优化
当你使用外部字体时往往需要引入额外的字体包，可是这个字体包通常会很大，而页面中只可能需要 100 个字，所以要对字体进行体积优化。
1.1 字体体积优化
这里推荐使用 font-spider-plus 完成
1.1.1 如何使用
第一步：首先全局安装插件
npm i font-spider-plus -g
mkdir index
cd index
mkdir fonts
第二步：创建 html 文件，并写入配置，将字体源文件放入 fonts 文件夹下
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
第三步： 执行生成文件命令
fsp local index.html
1.1.2 效果对比
	优化前	优化后
文件体积	8.4M	4kb
1.2 字体加载时优化
在字体加载的期间，浏览器页面是默认不展示文本内容的。即我们常说的 FOIT (Flash of Invisible Text)。 在现代浏览器中，FOIT 持续至多 3 秒，影响CLS指标，带来糟糕的用户体验。所以我们要关注如何平滑的加载字体。
1.2.1 font-display
他可以让 FOIT 的默认行为变为 FOUT (Flash of Unstyled Text)，即先会使用默认字体样式展示文本，字体加载完毕后再将文本的字体样式进行替换。
@font-face { 
  font-family: 'TencentSansW7'; 
  src: url('~@/assets/font/TencentSans-W7.ttf'); 
  font-display: optional; // 等待一小段时间（100ms），如果字体加载完则使用该字体，否则是否默认字体，并不再替换， 
}
1.2.2 内联字体
可以将字体文件转为 base64 的字符串，设置到 @font-face 里的 src 属性上：
@font-face {     
  font-family: 'TencentSansW7';     
  src: url('data:application/x-font-woff;charset=utf-8;base64,d09G…') format('woff2'); 
}
12. JS 体积优化
    2.1 优化 Babel 的使用
    2.1.1 @Babel/plugin-transform-runtime
    示例： 在我们使用 babel 编译以下代码时
    class Person{}
    编译后：
    function _instanceof(left, right) {
    if (right != null && typeof Symbol !== "undefined" && right[Symbol.hasInstance]) {
    return !!right[Symbol.hasInstance](left);
    }
    else {
    return left instanceof right;
    }
    }
    function \_classCallCheck(instance, Constructor) {
    if (!\_instanceof(instance, Constructor)) { throw new TypeError("Cannot call a class as a function"); }
    }
    var Person = function Person() {
    \_classCallCheck(this, Person);
    };
    可以看到 \_instanceof 和\_classCallCheck 都是 Babel 内置的 helpers 函数。如果每个 class 编译结果都在代码中植入这些 helpers 具体内容，对产出代码体积就会有明显恶化影响。 启用插件：
    {
    "plugins": [
    "@babel/plugin-transform-runtime",
    ]
    }
    这时使用启用@Babel/plugin-transform-runtime 插件后
    var \_interopRequireDefault = require("@babel/runtime/helpers/interopRequireDefault");
    var \_classCallCheck2 = \_interopRequireDefault(require("@babel/runtime/helpers/classCallCheck"));
    var Person = function Person() {  
     (0, \_classCallCheck2.default)(this, Person);
    };
    从上述代码我们可以看到，\_classCallCheck 作为模块依赖被引入文件中，基于打包工具的 cache 能力，从而减少了产出代码体积。 上述代码引用到的@babel/runtime 它含有 Babel 编译所需的一些运行时 helpers 函数，供业务代码引入模块化的 Babel helpers 函数，同时提供了 regenerator-runtime，对 generator 和 async 函数进行编译降级。
    2.1.2 按需引入 Babel-polyfill
    但是很多场景下我们可能只是使用了少量需要 polyfill 的 api，这个时候需要去做按需加载
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
    2.2 代码分割
    请求数和大小之间的权衡。HTTP2 普及后倾向于多文件，单个文件更小，提高缓存命中率和总体资源大小。
    ● 将依赖按照 package name + version 拆分，对于中小型项目来说，一个页面不会有太多依赖
    cacheGroups: {
    vendors: {
    test: /[\\/]node_odules[\\/]/,
    priority: 10,
    chunks: 'async',
    name(module: any) {
    // e.g. node_modules/.pnpm/lodash-es@4.17.21/node_modules/lodash-es
    const path = module.context.replace(/.pnpm[\\/]/, '');
    const packageName = path.match(
    /[\\/]node_modules[\\/](.*?)([\\/]|$)/,
      )[1];
      return `npm.${packageName
    .replace(/@/g, '\_at_')
    .replace(/\+/g, '\_')}`;
    },
    },
    },
    ● 将依赖按照基础库 + 被引用次数，对于大型项目来说，不会出现过多的 HTTP 请求，也比较好的利用了缓存
    cacheGroups: {
    lib: {
    name: 'lib',
    chunks: 'all',
    test: /(vue|vue-router|vuex|axios)/,
    priority: 40,
    enforce: true,
    },
    components:{
    name: 'components'
    priority: 30,
    minChunks: 1,
    reuseExistingChunk: true,
    chunks: 'async',
    },
    shared: {
    name:"shared",
    chunks: 'async',
    priority: 10,
    minChunks: 2,
    reuseExistingChunk: true,
    },
    },
    !!#ff0000 splitsChunks 各个配置间存在相互影响，与项目的实际结构有很强的相关性，无通用配置，根据项目结构多做取舍，找出拆包不符合预期的真正原因。
    ● 拆分运行时包
    ○ 运行时代码的内容由业务代码所使用到的特性决定，例如当 Webpack 检测到业务代码中使用了异步加载能力，就会将异步加载相关的运行时注入到产物中，因此业务代码用到的特性越多，运行时就会越大，有时甚至可以超过 1M 之多。

runtimeChunk: { name: 'runtime' } 3. CSS 优化
3.1 关键 CSS 内联
关键渲染路径（Critical Rendering Path，即 CRP）：是指优先显示与当前用户操作有关的内容。由于 CSS 会“间接”阻塞页面的解析，所以在这个过程中的 CSS 也被称为关键 CSS。 在性能优化上，我们会更关注关键渲染路径，而不一定是最快加载完整个页面。 这一阶段可以参考 critters。
3.2 谨慎使用 @import
CSS 提供了一个 @import 语法来加载外部的样式文件。然而，这会把你的请求变得串行化。 示例 比如 index.css 这个资源，页面上是这么引用的：

<link rel="stylesheet" href="index.css" />
而在 index.css 中引用了其他css
/* index.css */
@import url(other.css);
这样浏览器只有当下载了 index.css 并解析到其中 @import 时，才会再去请求 other.css。这是一个串行过程。
4. 图片优化
4.1 SVG
使用SVGO压缩 .svg格式图片 使用：
brew install svgo 
svgo *.svg
效果：平均减少50%左右的图片体积
4.2 webp / avif
使用合适的图片格式不仅能帮助你减少不必要的请求流量，同时还可能提供更好的图片体验，静态资源通常放在CDN上，这一步可直接在CDN开启即可，记得在开启前记录COS流量数据，方便优化前后对比。  CDN会通过HTTP 请求头中 accept 头部包含“image/avif”或“image/webp”来返回符合要求的图片。
4.3 Gif
可以在想要动图效果时使用视频，通过静音（muted）的 video 来代替 GIF。
六. 缓存
1. CDN
DNS 解析会将 CDN 资源的域名解析（通过CNAME）到 CDN 的负载均衡器上，负载均衡器可以通过请求的信息获取用户对应的地理区域，通过负载均衡算法，综合选择地理位置近、负载低的机器来提供服务. 核心特征:
● 缓存：缓存是把资源（COS）复制到CDN服务器里
● 回源：资源过期/不存在就向上层服务器（COS）请求并复制到CDN服务器里
1.1 CDN 容灾
需要注意的是由于各地理位置用户网络环境等原因，存储在CDN上的资源会出现加载失败的情况，导致部分静态资源加载失败，这时可考虑CDN容灾，可参考大佬们的文章：
● 前端网站容灾-CDN主域重试方案
● 美团CDN容灾方案
● CDN 可能会故障，但你的资源没必要

1.2 CDN 回源
提高 CDN 缓存命中率,降低回源率， 这一步我们通常借助 Webpack 等构件工具实现。
● 降低 CDN 的使用成本
● 降低资源加载时间，更低的回源率代表更多用户是访问到了缓存

通过 webpack 构建之后，生成对应文件名自动带上对应的 MD5 值。如果文件内容改变的话，那么对应文件哈希值也会改变，对应的 HTML 引用的 URL 地址也会改变，触发 CDN 服务器从源服务器上拉取对应数据，进而更新本地缓存。
1.2.1 指纹占位符
占位符名称 含义
chunkhash 根据 chunk 生成 hash 值，来源于同一个 chunk，则 hash 值就一样
contenthash 根据内容生成 hash 值，文件内容相同 hash 值就相同
ext 资源后缀名
folder 文件所在的文件夹
hash 每次 webpack 构建时生成一个唯一的 hash 值
name 文件名称
path 文件的相对路径
1.2.2 hash
● hash
Hash 是整个项目的 hash 值，其根据每次编译内容计算得到，每次编译之后都会生成新的 hash,即修改任何文件都会导致所有文件的 hash 发生改变，采用 hash 计算的话，每一次构建后生成的哈希值都不一样，即使文件内容压根没有改变。这样没办法实现缓存效果，我们需要换另一种哈希值计算方式。
● chunkhash
它根据不同的入口文件(Entry)进行依赖文件解析、构建对应的 chunk，生成对应的哈希值。我们在生产环境里把一些公共库和程序入口文件区分开，单独打包构建，接着我们采用 chunkhash 的方式生成哈希值，那么只要我们不改动公共库的代码，就可以保证其哈希值不会受影响
● contenthash
使用 chunkhash 存在一个问题，就是当在一个 JS 文件中引入 CSS 文件，编译后它们的 hash 是相同的，而且只要 js 文件发生改变 ，关联的 css 文件 hash 也会改变。 当使用 mini - css - extract - plugin 插件抽离 css 时, 可将 chunk 和 css 块都使用 contenthash 替换达到互不影响的作用;
output: {
filename: '[name].[contenthash:8].js',
path: path.resolve(\_\_dirname, 'dist-demo4'),
},
plugins: [
new CleanWebpackPlugin(),
new MiniCssExtractPlugin({
filename: '[name].[contenthash:8].css',
chunkFilename: '[name].[id].css'
}),
]
1.2.3 CDN 优化
1.2.3.1 开启 HTTP2.0
一个域名只使用一个 TCP 长连接和消除队头阻塞问题
利用多路复用，可以将请求分成一帧一帧的数据去传输，它允许同时通过同一个 TCP 连接发起多重的请求-响应消息，但在 TCP 层面还是有队头阻塞的问题，并由于只使用一个 TCP 长连接，TCP 队头阻塞的问题被放大了

1.2.3.2 开启 Gizp / Brotli

1.2.3.3 图片压缩
这部分参考静态资源 - 图片优化
七、 服务端渲染
关于服务端渲染的知识和实践，这里不展开讲了，感兴趣的同学可以参考之前我写过的三篇文章：
SSR 结合 Serverless 预研
基于 Next.js 12 和 Tailwind CSS 3.0 开发官网
流量业务官网 Serverless + SSR 实践总结

这里和大家一起聊一下 Island 架构和边缘计算：

1. Hydration
   首先了解一下什么是 Hydration，对于 SSR 应用而言，从用户请求到页面可交互经历以下几个阶段：
   ● 服务端通过 renderToString 直出 HTML。
   ● 浏览器下载所有相关的 JS，包括框架运行时代码、组件代码等。
   ● 解析和执行 JS。
   ● 构建出完整的渲染树，将渲染树和真实 DOM 关联匹配，并为 DOM 绑定事件。

上述的第四个阶段称为水合（Hydration），SSR 应用要面临的一个问题是随着页面的复杂性上升，Hydration 时间也会越来越长，在 Hydration 结束前页面都是不可交互的。 2. Island 架构
当一个页面中只有部分的组件交互，那么对于这些可交互的组件，我们可以执行 hydration 过程，因为组件之间是互相独立的。
而对于静态组件，即不可交互的组件，我们可以让其不参与 hydration 过程，直接复用服务端下发的 HTML 内容。 可交互的组件就犹如整个页面中的孤岛(Island)，因此这种模式叫做 Islands 架构。 3.边缘计算（ESR）
将页面进行动静拆分，将静态内容缓存在 CDN 先快速返回给用户，然后在边缘计算节点上发起动态内容的请求，之后将动态内容与静态部分以流的形式进行拼接，从而进一步提高了用户的首屏加载时间，尤其在边缘地区或者弱网环境也有能拥有很好的用户体验，此外还减少原先 SSR 服务器压力。
