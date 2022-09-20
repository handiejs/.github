「Handie」读作「ˈhændɪ」，源于「handy」，是一个尽可能地帮你把 CRUD 相关页面「配」出来而不是「写」出来的**渐进式配置驱动企业级中后台前端应用开发框架**。

本框架的核心目的是让业务开发者将关注点聚焦于业务本身而非技术细节，因此会搭配一系列封装好的中后台前端应用开发的基础设施，用相对简单的方式完成以往较为繁琐的工作。

虽然有一些推荐的思想、规范，但对于使用 Handie 来说并不是强制性的，故而可以在现有项目的基础上逐步迁移，而不用一下子伤筋动骨地大改。

## 起步

总的来说，Handie 的核心是平台无关的，但视图结构的渲染不可避免地依赖于 React、Vue 等视图库/框架，因而在它们的基础上进行了一定的封装与适配，从而形成了 [`handie-react`](https://github.com/handiejs/handie-react)、[`handie-vue`](https://github.com/handiejs/handie-vue) 等与具体视图库/框架绑定的「壳」。

为了方便说明，整个教程将基于 `handie-vue` 进行讲解。虽说如此，就算用 `handie-react` 或其他的「壳」，使用方式别无二致，无需担心。

### 前置知识

Handie 已经尽力去屏蔽一些底层技术的细节，也就是说，很理想的状况下只需要会一点 [JavaScript](https://developer.mozilla.org/en-US/docs/Learn/JavaScript) 相关知识就可以了。

然而，现实总是残酷的，实际面对的情景很可能是需求方不按套路出牌，界面的设计特殊化，这时就要根据选择而去了解些 [React](https://reactjs.org/)、[Vue](https://vuejs.org/) 及 [HTML](https://developer.mozilla.org/en-US/docs/Learn/HTML)、[CSS](https://developer.mozilla.org/en-US/docs/Learn/CSS) 等知识了。

### 准备工作

考虑到让 Handie 尽量轻量化，它只提供了使中后台前端应用能够配置化开发的底层能力而不内置任何资源型的「物料」，这些通过引入额外的 NPM 包来解决，比如[**部件**](https://www.yuque.com/handie/cookbook/qo4k0r)包 [`@handie/bulbasaur`](https://github.com/handiejs/bulbasaur)：

```bash
npm i -S handie-vue @handie/bulbasaur
```

由于部件包中基于 [Petals](https://petals.fxxk.design/) 采用**基于接口编程**的方式使用 UI 组件，不依赖任何具体的 UI 组件库，所以需要另外安装自己喜欢的 UI 组件库及相应的适配包。

以 [Element](https://element.eleme.cn/) 为例：

```bash
npm i -S element-ui @kokiri/element
```

在关键依赖都安装好之后，需将作为[**控件**](https://www.yuque.com/handie/cookbook/qo4k0r)的 UI 组件注册进系统，上面提到的 `@handie/bulbasaur` 才能正常运行——

为了兼容 ES Modules 的使用方式，首先要在项目的基础组件入口文件（如 `src/components/index.ts`）中把已安装的 UI 组件暴露出去。

需要注意的是，如无必要，暴露的是适配包中的而不是直接暴露 UI 组件库中的：

```typescript
// 导出全部
export * from '@kokiri/element';

// 或仅导出用到的
// export { Button, TextInput, TextArea, Select, Option } from '@kokiri/element';
```

然后，在项目入口文件（如 `src/main.ts`）中引入要被注册的 UI 组件：

```typescript
import { ComponentType, ComponentCtor, ComponentDescriptor, registerComponent } from 'handie-vue';

import * as renderers from 'handie-vue/dist/renderers'; // 渲染器
import * as controls from './components'; // 控件
import * as widgets from '@handie/bulbasaur'; // 部件

function convertToDescriptors(
  map: Record<string, ComponentCtor>,
  type: ComponentType = 'control',
): ComponentDescriptor[] {
  return Object.keys(map).map(name => ({ name, ctor: map[name], type }));
}

([] as ComponentDescriptor[])
  .concat(
    convertToDescriptors(controls),
    convertToDescriptors(widgets, 'widget'),
    convertToDescriptors(renderers, 'renderer'),
  )
  .forEach(descriptor => registerComponent(descriptor));
```

至此，准备工作已经做好，可以开始体验配置化开发啦！

## 「配」个页面出来

中后台系统的模式特征特别强，为了复用和简化开发，一些国内的 UI 组件库会另外提供更大粒度的用来支撑表格页、表单页等开发的 UI 组件，即所谓的「Pro」。

Handie 就是 [Fxxk Design](https://fxxk.design/) 或者说 Petals 在中后台场景的配套设施。

### 表格页

在大部分中后台前端应用中，要开发个表格页，代码大概是这样：

```vue
<template>
  <div class="AnimationListView">
    <!-- 查询条件 -->
    <div class="AnimationListView-search">
      <el-form class="AnimationListView-searchForm">
        <el-form-item label="标题">
          <el-input :value="title" @input="handleTitleChange" />
        </el-form-item>
      </el-form>
      <div class="AnimationListView-searchOperations">
        <el-button type="primary" @click="handleSearch">查询</el-button>
        <el-button @click="handleReset">重置</el-button>
      </div>
    </div>
    <!-- 表格行外操作 -->
    <div class="AnimationListView-operations">
      <el-button type="primary" @click="handleCreate">新增</el-button>
    </div>
    <!-- 数据展示 -->
    <div class="AnimationListView-data">
      <el-table class="AnimationListView-table" :data="tableData">
        <el-table-column prop="title" label="标题" />
        <el-table-column prop="description" label="简介" />
        <el-table-column prop="episodes" label="集数" />
        <!-- 表格行内操作 -->
        <el-table-column label="操作">
          <div slot-scope="{ row }">
            <el-button @click="handleEdit(row)">编辑</el-button>
          </div>
        </el-table-column>
      </el-table>
      <el-pagination
        class="AnimationListView-pagination"
        :page-size="pageSize"
        :current-page="pageNumber"
        :total="total"
        @size-change="handlePageSizeChange"
        @current-change="handlePageNumberChange"
      />
    </div>
  </div>
</template>

<script lang="ts">
import { Vue, Component } from 'vue-property-decorator';

@Component
export default class AnimationListView extends Vue {
  private tableData: Record<string, any>[] = [];

  private title: string = '';

  private pageSize: number = 20;
  private pageNumber: number = 1;
  private total: number = 0;

  private async getList(): Promise<void> {
    // 发起获取列表数据的请求
  }

  private handleTitleChange(value: string): void {
    this.title = value;
  }

  private handleSearch(): void {
    this.getList();
  }

  private handleReset(): void {
    this.title = '';

    this.getList();
  }

  private handlePageSizeChange(pageSize: number): void {
    this.pageSize = pageSize;

    this.getList();
  }

  private handlePageNumberChange(pageNumber: number): void {
    this.pageNumber = pageNumber;

    this.getList();
  }

  private handleCreate(): void {
    this.$router.push({ name: 'AnimationNewForm' })
  }

  private handleEdit(record: Record<string, any>): void {
    this.$router.push({ name: 'AnimationEditForm', params: { id: record.id } });
  }

  private created(): void {
    this.getList();
  }
}
</script>

<style lang="scss" scoped>
.AnimationListView {
  // 样式规则集
}
</style>
```

上面的示例代码，可以说是最简功能的最小实现了，省去了很多细节没写。即便如此，仍然「写」了百余行……

下面借助于 Handie 的能力「配」一个具有相同功能的表格页：

```typescript
import { ObjectViewContext, createView } from 'handie-vue';

import context from '../context';

export default createView(context, {
  name: 'AnimationListView',
  category: 'list',
  renderType: 'table',
  // 数据列
  fields: [
    { name: 'title', label: '标题', dataType: 'string' },
    { name: 'description', label: '简介', dataType: 'text' },
    { name: 'episodes', label: '集数', dataType: 'int' },
  ],
  // 操作按钮
  actions: [
    {
      text: '新增',
      context: 'free',
      primary: true,
      execute: (_, vm) => vm.$router.push({ name: 'AnimationNewForm' }),
    },
    {
      text: '编辑',
      execute: (context: ObjectViewContext, vm) =>
        vm.$router.push({
          name: 'AnimationEditForm',
          params: { id: context.getFieldValue('id') },
        }),
    },
  ],
  // 查询条件
  search: {
    filters: [{ name: 'title', label: '标题' }],
  },
});
```

通过对比应该可以明显地感受到，下面的代码比上面的减少了很多信息噪音，能够很清晰地看出这个页面都显示哪些字段，有什么操作和查询条件等。

### 表单页

如果要「写」出来一个表单页，代码量会比表格页多很多，限于篇幅就不写具体示例了。而要「配」一个表单页，其代码量与表格页比较相近，甚至更少：

```typescript
import { createView } from 'handie-vue';

import context from '../context';

export default createView(context, {
  name: 'AnimationFormView',
  category: 'object',
  renderType: 'form',
  fields: [
    { name: 'title', label: '标题', dataType: 'string' },
    { name: 'description', label: '简介', dataType: 'text' },
    { name: 'episodes', label: '集数', dataType: 'int' },
  ],
});
```

## 业务向的模块系统

除了配置化开发，Handie 的另一大特色就是针对业务的模块化开发。

### 自造的缘由

也许你会疑问：「JavaScript 不是已经有了语言级别的模块系统 ES Modules 了吗？为啥还要自己搞一套？」

ES Modules 是基于文件（路径）的，确定性很高，因此可以在编译时做静态分析等事情。

然而，正是因为这种确定性，当我们想要在运行时动态替换既有应用中的某个部分时，要么无法做到，要么成本很高，比如：无缝切换数据源或 UI 组件库。

另外，也由于 ES Modules 是基于文件（路径）的，不具备业务语义，并且不能对业务方面的依赖做约束。使用 Handie 内置的模块系统配合模块内的 ES Modules 引用规则，可以从业务角度进行更加严格的模块依赖管控。

### 模块描述器

所谓的「模块描述器」就是一个 JavaScript 对象，通过它可以了解到当前模块依赖于哪些业务模块和控件，以及有哪些资源可被外部模块使用：

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

### 模块上下文

在上文介绍配置化开发时，表格页和表单页的示例代码中有一段相同的代码：

```typescript
import context from '../context';
```

这里的 `context` 就是「模块上下文」——一个用来获取模块级资源的 JavaScript 对象，如：获取控件构造函数，获取依赖模块的资源，发送请求等。

## 准备好了吗？

在本篇教程中只是着重并简要地介绍下 Handie 最核心的特色功能，教程的其余部分将会详细地讲解完整机制，以及如何应对变化的业务，所以请耐心地读完全部教程！
