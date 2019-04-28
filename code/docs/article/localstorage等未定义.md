---
classify: 坑货集合
tags: [Vuepress, error, undefined, SSR, Node.js, HTML]
---
## LocalStorage is undefined
### 背景

在用 vuepress 搭建本博客时，文章列表采用的 **\$site.pages**然后进行排序筛选，然后为了方便，我把数组存到了 SessionStorage 里，然后再打包时遇到报错，说 SessionStorage 未定义，然后我改成 LocalStorage，还是会报这个错，然后百度说加个 window 对象，采用 window.LocalStorage，然后报错就变成了 window 未定义。在写的时候我就感觉不靠谱，因为 node 环境下是没有 window 对象的，然后翻到了一个靠谱的解释

**ssr 在服务端生命周期 mounted 之前没有 LocalStorage 这个浏览器对象，**

突然想到之前在[vuepress 的官方文档](https://vuepress.vuejs.org/zh/guide/using-vue.html#%E6%B5%8F%E8%A7%88%E5%99%A8%E7%9A%84-api-%E8%AE%BF%E9%97%AE%E9%99%90%E5%88%B6)也看到过，说代码要符合通用代码规范，

**当你在开发一个 VuePress 应用时，由于所有的页面在生成静态 HTML 时都需要通过 Node.js 服务端渲染，因此所有的 Vue 相关代码都应当遵循 编写通用代码 的要求。简而言之，请确保只在 beforeMount 或者 mounted 访问浏览器 / DOM 的 API。**

最后，我把 created()里面的代码放到了 mounted()里，然后打包 ok 了。。。吗？

### 坑

我也以为这样就行了，然后发现虽然能通过打包，但是运行时还是会有一些报错，虽然不是提示 Storage 方面的了，但是还是跟他相关的，但是我又懒得去排查了，所以我把所要存的列表数据，放到了 Layout 组件里，然后通过 props 传给子组件，子组件在进行筛选，搞定。
