## 可复用级别

按照代码的可复用范围，可分为以下几个级别：开源级（三方包） > 公司级（二方包） > 部门级（二方包） > 项目级 > 模块级 > 视图级。根据组织和项目的规模，公司级和部门级可以合并，项目级与模块级可以合并。

在实际的中后台前端项目中要简化层级，需将开源级、公司级和部门级合并至项目级中，也就是将开源级、公司级和部门级的 NPM 包在项目的公共设施中重新导出，如在 `components/widget/index.ts` 文件中：

```typescript
export * from '@handie/bulbasaur';
```

因此，项目中资源的可复用级别被简化为项目级、模块级与视图级这三个。

## 控件定制

TBD

## 部件定制

「描述器」和「部件」是 Handie 体系中对使用者来说很是关键的两个概念——前者的主要职责是为配置化开发提供入口，并起到控制模块边界与 I/O 的作用；后者则是配置化开发在表现层的支撑，同时也是表现层的重要扩展点。

在《[编写模块](/guides/defining-modules)》中以「如何去定义一个模块」的角度在描述器的层面讲解了在定义一个模块时需要哪些资源以及怎样去组织它们；在此将说明当已有的部件无法满足需求时如何去自定义。

### 理解「部件」

「部件」是承载了 Handie 所提供的几个抽象，并应用了上下文的 UI 组件，有视图、字段、动作、搜索和过滤器这五类——「部件」就是 UI 组件，但是与 Handie 的上下文机制深度结合。

在自定义部件时会派上用场的上下文有视图上下文、模块上下文和搜索上下文——

#### 视图上下文

不像大部分中后台前端应用那样采用全局的集中式状态管理，在 Handie 中的状态管理方案是以视图为中心的分布式管理。

根据数据结构的不同，视图上下文细分为列表类视图上下文和对象类视图上下文，常用 API 如下：

| 所属上下文 | 方法 | 说明 |
| --- | --- | --- |
| 通用 | `getModuleContext()` | 获取模块上下文 |
|  | `setDataSource(data)`<br>`getDataSource()` | 设置/获取数据源，即最初最原始的数据<br>设置数据源后会触发 `dataChange` 事件，并将数据源作为载荷传入 |
|  | `setValue(value)`<br>`getValue()` | 设置/获取当前值，即在不断变化的活动值：在列表类视图中是选中的行记录；在对象类视图中是编辑中的字段值集合<br>设置当前值后会触发 `change` 事件，并将当前值作为载荷传入；若当前值是第一次设置，会额外触发 `ready` 事件 |
|  | `setBusy(busy)`<br>`getBusy()` | 设置/获取忙碌状态，用来调控数据相关逻辑以及在界面中显示「加载中」效果<br>设置忙碌状态后会触发 `busyChange` 事件，并将当前是否忙碌作为载荷传入 |
| 列表类视图上下文 | `getSearchContext()` | 获取搜索上下文 |
|  | `load()`<br>`reload()` | 加载/重载列表数据<br>加载后会触发 `totalChange` 事件，并将数据总条数作为载荷传入 |
|  | `setCurrentPage(current)`<br>`getCurrentPage()` | 设置/获取列表当前页<br>设置当前页后会触发 `currentPageChange` 事件，并将页码作为载荷传入 |
|  | `setPageSize(size)`<br>`getPageSize()` | 设置/获取列表每页显示条数<br>设置每页显示条数后会触发 `pageSizeChange` 事件，并将每页条数作为载荷传入 |
|  | `getList(params, success?, fail?)` | 获取列表数据的便捷方法 |
|  | `deleteOne(params, success?, fail?)` | 删除单条记录的便捷方法 |
|  | `deleteList(params, success?, fail?)` | 删除多条记录的便捷方法 |
| 对象类视图上下文 | `getParent()` | 获取父级视图上下文（列表类视图上下文） |
|  | `setFieldValue(name, value)`<br>`getFieldValue(name)` | 设置/获取字段值<br>设置字段值后会触发 `fieldChange` 事件，并将被设置的字段键与值组合成对象作为载荷传入 |
|  | `submit()` | 对字段值进行校验并提交当前值，具体是如何处理被提交的当前值，需要外部监听 `submit` 事件进行后续处理 |
|  | `reset()` | 重置当前值到默认值（由各字段的数据类型定义决定）<br>重置后会触发 `reset` 事件，并将重置后的值作为载荷传入 |
|  | `getOne(params, success?, fail?)` | 获取单条记录的便捷方法 |
|  | `insert(data, success?, fail?)` | 插入数据的便捷方法 |
|  | `update(data, success?, fail?)` | 更新数据的便捷方法 |

在渲染部件时已经将视图上下文注入进去，在视图部件、字段部件和动作部件中可通过 `this.context` 访问，在搜索部件和过滤器部件中则用 `this.viewContext` 获取。

#### 模块上下文

模块上下文提供了访问模块级别资源的能力，这些资源都是[通过模块描述器进行声明](/guides/getting-started)的。

模块上下文不能直接获取，需要经视图上下文的 `getModuleContext()` 方法间接访问，其 API 为：

| 方法 | 说明 |
| --- | --- |
| `getModuleName()` | 获取模块名 |
| `getModel()` | 获取模块绑定的[模型定义](/guides/defining-modules) |
| `getDependencies(refPath?)` | 获取当前模块所依赖的其他模块资源 |
| `getComponents()` | 获取模块所使用的控件的构造函数 |
| `execute(actionName, params?, success?, fail?)` | 执行[服务端动作](/guides/defining-modules)，当所执行动作不需要传参时，第二个参数为逻辑成功的回调，第三个参数为逻辑失败的回调 |

#### 搜索上下文

搜索上下文是用来维护列表类视图的过滤条件的，有着与对象类视图上下文相似的 API：

| 方法 | 说明 |
| --- | --- |
| `setValue(value)`<br>`getValue()` | 获取/设置全部过滤条件<br>设置过滤条件后会触发 `change` 事件，并将全部过滤条件作为载荷传入；若是第一次设置，会额外触发 `ready` 事件 |
| `setFilterValue(name, value)`<br>`getFilterValue(name)` | 设置/获取过滤器值<br>设置过滤器值后会触发 `filterChange` 事件，并将被设置的过滤器键与值组合成对象作为载荷传入 |
| `submit()` | 提交过滤条件，具体是如何处理被提交的过滤条件，需要外部监听 `submit` 事件进行后续处理 |
| `reset()` | 重置过滤条件到默认值<br>重置后会触发 `reset` 事件，并将重置后的值作为载荷传入 |

在搜索部件中可用 `this.context` 直接访问搜索上下文，其他情况需通过列表类视图上下文的 `getSearchContext()` 方法进行获取。

### 自定义部件

看过《[编写模块](/guides/defining-modules)》中的「视图定义」的话，可得知能够通过视图描述器的 `renderType` 和 `widget` 这两个属性去指定想要渲染的部件——`widget` 用于模块级与视图级的部件，而 `renderType` 则用于其他级别的，它们不可同时使用。

上述两个属性不是视图描述器所特有的，字段、动作、搜索和过滤器的描述器都有它们，故而都可指定想要渲染的部件！

#### 命名规则

在自定义部件时要注意部件的命名，尤其是公司级、部门级和项目级的，需要遵照一定的命名规则才能在赋值给 `renderType` 属性时被查找到。

虽然不同种类的部件在命名上有所差异，但它们的基本模式是 `渲染类型 + 部件种类 + Widget`。

在给部件命名时 `渲染类型` 和 `部件种类` 都是大驼峰式（Pascal Case），但在赋值给描述器的 `renderType` 属性时，需要将 `渲染类型` 变为肉串式（Kebab Case）。

视图、动作和搜索部件的命名与基本模式一致；字段部件的命名模式是 `渲染类型 + 编辑状态 + 数据类型 + 部件种类 + Widget`；而过滤器部件则是 `渲染类型 + 数据类型 + 部件种类 + Widget`。

示例如下表所示：

| 种类 | 命名 | 说明 |
| --- | --- | --- |
| 视图 | `TableViewWidget` | 表格视图部件 |
|  | `FormViewWidget` | 表单视图部件 |
| 字段 | `InputEditStringFieldWidget` | 用于字符串数据的输入框编辑态字段部件 |
|  | `InputReadStringFieldWidget` | 用于字符串数据的输入框只读态字段部件 |
|  | `SwitchEditBooleanFieldWidget` | 用于布尔型数据的开关编辑态字段部件 |
|  | `SwitchReadBooleanFieldWidget` | 用于布尔型数据的开关只读态字段部件 |
| 动作 | `ButtonActionWidget` | 按钮动作部件 |
|  | `LinkActionWidget` | 链接动作部件 |
| 搜索 | `FormSearchWidget` | 表单搜索部件 |
| 过滤器 | `InputStringFilterWidget` | 用于字符串数据的输入框过滤器部件 |
|  | `SelectEnumFilterWidget` | 用于枚举数据的下拉列表过滤器部件 |


需要说明的是，要严格遵守命名规则的是部件在被 `export` 导出并被注册进系统中时的名字。

#### 编写部件

由于部件有很多种，这里就不一一讲述，只拿会被自定义频率较高的字段部件稍作演示——

用于字符串数据的输入框编辑态字段部件：

```typescript
import { CreateElement, VNode } from 'vue';
import { Component } from 'vue-property-decorator';

import { getControl } from 'handie-vue';
import { StringFieldHeadlessWidget } from 'handie-vue/dist/widgets';

@Component
export default class InputEditStringFieldWidget extends StringFieldHeadlessWidget {
  private render(h: CreateElement): VNode {
    return h(getControl('TextInput'), {
      props: {
        value: this.value, // 由对象类视图上下文下发的字段值
        placeholder: this.getPlaceholder(), // 根据字段的 `config` 所生成的占位文本
      },
      on: {
        input: this.onChange, // 将用户输入的数据变化汇总到对象类视图上下文中
      }
    });
  }
}
```

用于字符串数据的输入框只读态字段部件：

```vue
<template>
  <!-- 由对象类视图上下文下发的字段值 -->
  <span>{{ value }}</span>
</template>

<script lang="ts">
import { Component } from 'vue-property-decorator';
import { StringFieldHeadlessWidget } from 'handie-vue/dist/widgets';

@Component
export default class InputReadStringFieldWidget extends StringFieldHeadlessWidget {}
</script>
```

上面所示的是项目级部件的写法，通过字段描述器的 `renderType` 属性指定；若要自定义模块级或视图级的用字段描述器的 `widget` 属性指定的部件，则编辑态和只读态都是同一个部件：

```typescript
import { CreateElement, VNode } from 'vue';
import { Component } from 'vue-property-decorator';

import { getControl } from 'handie-vue';
import { StringFieldHeadlessWidget } from 'handie-vue/dist/widgets';

@Component
export default class InputStringFieldWidget extends StringFieldHeadlessWidget {
  private render(h: CreateElement): VNode {
    return this.field.readonly === true // 字段是否只读
      ? h('span', this.value)
      : h(getControl('TextInput'), {
      props: {
        value: this.value,
        placeholder: this.getPlaceholder(),
      },
      on: {
        input: this.onChange,
      }
    });
  }
}
```

更多部件的自定义请见《[部件基类](/guides/prx3th)》。

## 动作沉淀

TBD

## 数据类型扩展

TBD
