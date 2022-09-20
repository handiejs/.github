为了让初识 Handie 的人能够快速感知到大体轮廓，在《[初次见面，请多指教！](/guides/intro)》中着重并简要地介绍了最核心的特色功能——配置化开发与面向业务的模块化开发。

光凭那一篇还无以令对 Handie 感兴趣的人真的配出一个可运行的页面，因而有了这一系列「入门教程」。快来跟随我的脚步，一步一步学会如何使用 Handie 吧！

## 运行环境

Handie 的核心是平台无关的，也就是说，理论上 Handie 可用于任何支持 JavaScript 的运行环境。然而，在实际使用时或多或少会依赖于运行环境中渲染视图单元的库/框架，故而会针对具体场景在核心之上进行一定的封装与适配，从而形成了 `handie-vue` 等与运行环境绑定的「壳」。

考虑到让「壳」尽量轻量化，它只提供了使中后台前端应用能够配置化开发的底层能力而不内置任何资源型的「物料」，这些通过引入额外的 NPM 包来解决。

为方便说明，整套教程基于 `handie-vue` 进行讲解。虽说如此，就算用其他的「壳」，使用方式别无二致，无需担心。

### 安装依赖

安装与 Vue 绑定的「壳」和基础部件包 `@handie/bulbasaur`：

```bash
npm i -S handie-vue @handie/bulbasaur
```

再安装自己喜欢的 UI 组件库及相应的适配包，以 Element 为例：

```bash
npm i -S element-ui @kokiri/element
```

### 支持组件

由于基础部件包中是基于 Petals 采用基于接口编程的方式使用 UI 组件，不依赖任何具体的 UI 组件库，所以需要另外安装符合 Petals 接口规范的 UI 组件库或适配包，如上面提到的 `@kokiri/element`。其他情况可以按照常规方式使用 UI 组件。

下面是符合 Petals 接口规范的 Vue 组件库或适配包：

| UI 组件库 | 适配包 | 说明 |
| --- | --- | --- |
| `kokiri` | - | 直接基于 Petals 开发，并支持所有 Petals 中定义的接口 |
| `element-ui` | `@kokiri/element` | - |
| `view-design` | `@kokiri/view-ui` | - |


有适配包的 UI 组件库需要两者搭配使用，适配包是在 UI 组件库中已有的 UI 组件的基础上将属性、事件等适配到 Petals 中所定义的接口。

为了日后可以无缝替换 UI 组件库以及做测试等，建议在开发时使用适配包中导出的 UI 组件，并采用 Handie 推荐的方式开发。

## 准备资源

这里所说的「资源」，是让基于 Handie 创建的应用能够正常运作的基石。像控件、部件、渲染器等 UI 组件，及模块定义、模型定义、视图定义、动作定义、主题配置等元数据，都是「资源」，也可称为「物料」。

### UI 组件

在 Handie 当前的体系中，根据用途的不同，UI 组件分为[控件、部件和渲染器](https://www.yuque.com/handie/cookbook/qo4k0r)这三类，分别可以粗略地理解为：

- 业务无关且不与 Handie 的机制绑定的是「控件」，像 UI 组件库中的就算是这一类；
- 与 Handie 所提供的抽象（如[动作](https://www.yuque.com/handie/cookbook/txgfzp)、[视图与字段](https://www.yuque.com/handie/cookbook/sgvck8)等）密切相关，并应用了[上下文](https://www.yuque.com/handie/cookbook/qphgck)的是「部件」；
- 通过元数据查找并渲染部件的是「渲染器」。

Handie 官方已经提供了一些现成的 UI 组件，可以拿来即用——

一般来说，项目中会有一个专门用来存放 UI 组件的 `components` 文件夹，在其下为控件、部件和渲染器分别创建对应的文件或文件夹。

以控件为例，如果项目相对简单，不需要自定义项目级的控件，只创建一个 `control.ts` 文件即可，否则就要创建 `control` 文件夹并用 `control/index.ts` 文件统一导出所有控件：

```typescript
export * from '@kokiri/element';
```

部件与渲染器基本同理：

```text
components
   ├── widget
   │   └── ...
   ├── control.ts
   └── renderer.ts
```

在 `renderer.ts` 文件中把渲染器全部导出：

```typescript
export * from 'handie-vue/dist/renderers';
```

但部件相对特殊一点，因为已提供的部件比较容易出现不满足业务需求的情况，所以提供了仅保留与上下文交互能力的「无头」部件，以便自定义部件。

这样一来，部件所对应的就不是 `widget.ts` 文件，而是 `widget` 文件夹，并且 `widget` 文件夹下还要建一个能够被单独引入的 `base.ts` 文件：

```text
components
   ├── widget
   │   ├── base.ts
   │   └── index.ts
   ├── control.ts
   └── renderer.ts
```

其中，`widget/base.ts` 文件的内容为：

```typescript
export * from 'handie-vue/dist/widgets';
```

而 `widget/index.ts` 文件的内容则是：

```typescript
export * from '@handie/bulbasaur/dist/filter';
export * from '@handie/bulbasaur/dist/search';
export * from '@handie/bulbasaur/dist/action';
export * from '@handie/bulbasaur/dist/field';
export * from '@handie/bulbasaur/dist/view';
```

若无任何非 Handie 官方提供的部件的话，也可以这样导出：

```typescript
export * from '@handie/bulbasaur';
```

把 UI 组件都准备好后，在 `components/index.ts` 文件中将它们转化为 UI 组件描述器并导出：

```typescript
import { ComponentType, ComponentCtor, ComponentDescriptor } from 'handie-vue';

import * as controls from './control';
import * as widgets from './widget';
import * as renderers from './renderer';

function convertToDescriptors(
  map: Record<string, ComponentCtor>,
  type: ComponentType = 'control',
): ComponentDescriptor[] {
  return Object.keys(map).map(name => ({ name, ctor: map[name], type }));
}

export default ([] as ComponentDescriptor[]).concat(
  convertToDescriptors(controls),
  convertToDescriptors(widgets, 'widget'),
  convertToDescriptors(renderers, 'renderer'),
);
```

需要注意的是，**`@/components` 仅用于收集 UI 组件相关资源，不可在开发时使用；若要在开发时用 ES Modules 直接引入，需用 `@/components/control`、`@/components/widget`、`@/components/renderer` 这几个路径。**

### 元数据

#### 模块定义

用来进行模块定义的模块描述器是 Handie 得以运行的必不可少的元数据——如果把它忘了，你将会在网页中看到白花花的一片——什么也没有！

模块描述器是一个比较简单的 JavaScript 对象，通过它能够很直观地知道依赖了哪些模块，有哪些可被其他模块使用的资源，模块内用了哪些 UI 组件（不包括用 ES Modules 直接引入的），以及模块所拥有的视图、服务端动作等——可以说，模块描述器相当于物料清单。

在 Handie 推荐的[目录结构划分模式](https://www.yuque.com/handie/cookbook/directory-structure)中，领域/业务模块都存放在 `domain` 文件夹下；但在常见的中后台前端项目目录结构中，可以把 `pages` 或 `views` 文件夹当作 `domain` 文件夹来用。

每个领域/业务模块文件夹下都要有一个用来导出模块描述器的 `index.ts` 文件：

```typescript
import { ModuleDescriptor } from 'handie-vue';

import { AnimationList } from './widgets';

export default {
  // 模块名，需要全局唯一
  name: 'animation',
  // 所依赖的其他模块的资源
  imports: ['genre.services.getList'],
  // 提供给外部模块使用的资源
  exports: {
    widgets: { AnimationList },
  },
  // 模块内依赖的控件
  components: {
    XIcon: 'Icon',
    XButton: 'Button',
    Wait: true,
    Ellipsis: true,
  },
} as ModuleDescriptor;
```

模块描述器最终会被 Handie 用于模块依赖管控，生成并注入模块上下文，以及查找并创建视图等。

#### 动作定义

TBD

#### 主题配置

TBD

## 创建应用

要创建应用，需要从 `handie-vue` 或 `handie-vue/dist/vendors/handie` 中引入 `createApp`，两者的主要区别是：前者在后者的基础上封装了 Vue 应用的初始化逻辑，包括对 Vue Router 和 Vuex 等的处理；而后者则是单纯地统一注册 UI 组件、元数据等资源。

```typescript
createApp({
  components,
  metadata: {
    actions, // 客户端动作
    modules, // 领域/业务模块
  },
  theme: {
    icon: {
      providers: {
        // ...
      },
    },
    behavior: {
      // ...
    },
  },
  /**
   * 以下内容只有在直接从 `handie-vue` 中引入 `createApp` 时有效
   */
  plugins: [Vuex, ...plugins],
  creators: { store: moduleTree => new Vuex.Store({ modules: moduleTree }) },
  el: '#app',
  routes,
});
```

上段代码可以直接写在应用入口文件 `main.ts` 中，也可以写在另外的文件中，如 `entry.ts`，然后在 `main.ts` 文件中引入：

```typescript
import './entry';
```

目前 Handie 只支持单应用（计划中要支持多应用），因此所谓的「创建」应用实际上纯粹是「初始化」应用。
