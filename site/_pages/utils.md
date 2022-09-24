---
title: 工具函数
layout: guide
css:
  - pages/post
  - pages/doc
js:
  - initializers/time
  - initializers/lazyload
repo: api
---

为日常应用开发的便利，Handie 中包含并全量导出了工具包 [`@ntks/toolbox`](https://www.npmjs.com/package/@ntks/toolbox) 中的全部函数，明细及用法详见其[使用手册](https://ntks.ourai.ws/projects/toolbox/)。

以下为 Handie 自身提供的工具函数——

## 核心

使 Handie 能够运转起来而必不可少的。

### `createApp(descriptor)`

创建并初始化一个应用实例，通过此函数可以替代 Vue、React 等视图层库/框架自带的应用初始化方式以及 `registerComponent`、`registerAction` 和 `registerModules` 等函数。

{% highlight ts %}
import { createApp } from 'handie-vue';

import components from '@/shared/components';
import modules from '../domain';
import actions from './actions';
import routes from './routes';
import theme from './theme';

createApp({
  components,
  metadata: { actions, modules },
  theme,
  routes,
});
{% endhighlight %}

## 兼容

Handie 是「渐进式配置驱动企业级中后台前端应用开发框架」，既然是「渐进式」，就必须具备低成本与既有项目相融合的能力，也就是没有或尽可能低的侵入性。

为此而提供了一些用于兼容的函数，在项目完全迁移到 Handie 之后，即当完全元数据化/配置化时，能够以较低的成本移除。

### `createModuleContext(nameOrDescriptor)`

创建模块上下文。

{% highlight ts %}
import { createModuleContext } from 'handie-vue';

import { MODULE_NAME } from './helper';
import * as actions from './repository';

export default createModuleContext({ moduleName: MODULE_NAME, actions });
{% endhighlight %}

### `createView(moduleContext, descriptor)`

创建视图 UI 组件。

{% highlight ts %}
import { createView } from 'handie-vue';

import context from '../../context'; // 模型上下文

import TitleField from './TitleField.vue';
import EpisodesField from './EpisodesField.vue';

export default createView(context, {
  name: 'AnimationListView',
  type: 'list',
  renderType: 'table',
  config: { operationColumnWidth: 250 },
  fields: [
    { name: 'title', label: '标题', widget: TitleField, config: { width: '300' } },
    { name: 'description', label: '简介' },
    {
      name: 'episodes',
      label: '集数',
      widget: EpisodesField,
      config: { width: '60', align: 'center' },
    },
  ],
  actions: [
    { name: 'gotoCreateFormView', authority: 'animation:edit', primary: true },
    { name: 'deleteList', authority: 'animation:edit' },
    'gotoDetailView',
    { name: 'gotoEditFormView', authority: 'animation:edit' },
    { name: 'deleteOne', authority: 'animation:edit' },
  ],
  search: {
    filters: [
      { name: 'title', label: '标题' },
      { name: 'description', label: '简介' },
    ],
  },
});
{% endhighlight %}

## 辅助

### `getAppHelper()`

获取应用助手。

### `getDependencies(moduleName, refPath?)`

获取指定模块在模块描述器中声明的依赖。

### `getComponents(moduleName, newComponents?)`

获取指定模块在模块描述器中声明的用到的 UI 组件。

{% highlight ts %}
import { Vue, Component } from 'vue-property-decorator';
import { getComponents } from 'handie-vue';

import { MODULE_NAME } from './helper';

@Component({
  components: getComponents(MODULE_NAME),
})
export default class TestWidget extends Vue {}
{% endhighlight %}

## 扩展

基于 Handie 底层机制针对业务应用进行适配与增强。

### `registerDataType(descriptor)`

## 定制

主要用于在 [`@handie/runtime-core`](https://www.npmjs.com/package/@handie/runtime-core) 的基础上开发新的视图库适配包或 starter 等。
