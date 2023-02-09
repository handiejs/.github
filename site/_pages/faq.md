---
title: 常见问题
layout: guide
css:
  - pages/post
  - pages/doc
js:
  - initializers/time
  - initializers/lazyload
repo: cookbook
---

以下整理了一些在使用 Handie 时可能常见或比较有特点的问题以及官方答复，在提问之前建议找找有没有类似的问题。**若无特殊说明，与视图库/框架相关的示例代码用 Vue 编写。**

## 想复用由 `createView()` 创建的视图时页面显示空白

假设在 `animation` 模块下有个用 `createView()` 创建的动画列表：

{% highlight ts %}
// `domain/animation/views/animation-list/index.ts`

// ...

export default createView(context, {
  name: 'AnimationListView',
  category: 'list',
  renderType: 'table',
  fields: [/* ... */],
  actions: [/* ... */],
  search: [/* ... */],
});
{% endhighlight %}

并且在路由配置中对其进行了关联：

{% highlight ts %}
// `entry/routes/animation.ts`

// ...

export default [
  {
    name: 'animationList',
    path: 'animations',
    component: () => import("@/domain/animation/views/animation-list"),
  },
];
{% endhighlight %}

在另外一个页面中的某个区域也要显示动画列表，想要进行复用，于是乎：

{% highlight ts %}
// `domain/animation/views/index.ts`

export { default as AnimationListView } from './animation-list';
{% endhighlight %}

在浏览器页面刷新后，会看到原本正常显示的动画列表页的主体区域一片空白……

造成这个问题的根本原因是代码执行顺序——

在应用入口文件中 `createApp()` 时会对 Handie 运行所必需的各种资源进行注册：

{% highlight ts %}
// `entry/index.ts`

// ...

import components from '../shared/components';
import modules from '../domain';
import routes from './routes';

// ...

createApp({
  components,
  metadata: { modules },
  routes,
  // ...
});
{% endhighlight %}

然而，资源注册之前在 `import modules from '../domain';` 时就已经执行了 `animation` 模块下创建动画列表的 `createView()`，这时是找不到要被渲染的公用视图部件的，就显示了空白。

复用动画列表的正确方式是：

1. 用返回视图描述器的工厂函数替代 `createView()`；
2. 创建一个代理用的 UI 组件，里面用[视图渲染器](/renderers/view-renderer/)去渲染改造好的视图，并将路由配置指向该组件；
3. 在其他要复用的地方同样使用视图渲染器去渲染。
