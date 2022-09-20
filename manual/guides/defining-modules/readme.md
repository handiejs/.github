以领域/业务模块为中心的元数据。

## 模块构成

一个完整的领域/业务模块包含了模型、视图、动作、部件和模块描述器。其中，视图、动作与模块描述器是不可或缺的，它们是保障 Handie 能够运作的基本条件，其他的可以视情况决定有无。

按照 Handie 推荐的[目录结构划分](/guides/directory-structure)来看，上述构成要素分别由以下文件夹或文件来承载：

| 文件夹/文件 | 构成要素 |
| --- | --- |
| `views` | 视图 |
| `widgets` | 部件 |
| `repository.ts` 或 `services.ts` | 服务端动作 |
| `model.ts` | 模型 |
| `index.ts` | 模块描述器 |


## 动作定义

简单来说，「动作」就是一段逻辑。在 Handie 的体系中，「服务端动作」就是 HTTP 数据请求服务，而「客户端动作」是传入了视图上下文的前端逻辑。

### 服务端动作

#### 返回结构

在如何发起 HTTP 请求这件事上 Handie 并没有什么限制，但对请求的返回结构有一定的要求。若后端无法直接返回符合要求的结构，则需要前端接到响应时统一处理。

以 [Axios](https://axios-http.com/) 为例，添加个响应[拦截器](https://axios-http.com/docs/interceptors)去转换：

```typescript
import { AxiosResponse } from 'axios';
import { ResponseResult } from 'handie-vue';

import httpClient from '@/utils/http';

function normalizeResponse(response: AxiosResponse): ResponseResult {
  return {
    success: true, // 是否逻辑成功
    message: '', // 逻辑失败时的提示信息
    code: '', // 逻辑失败时的错误编码
    data: [], // 逻辑成功时的返回数据
    // 逻辑成功时的附加数据，分页等信息需要挂在这个上面：
    // `total` 是总条数，
    // `pageNum` 是当前页，
    // `pageSize` 是每页条数
    extra: {},
  };
}

httpClient.interceptors.response.use(
  response => {
    return normalizeResponse(response);
  },
  error => {
    return Promise.reject(error);
  },
);
```

这样就能保证每个通过 `httpClient` 发出的请求的最终返回结构都是符合 Handie 要求的。如此一来，在使用模块上下文执行服务端动作时就能够便捷地传入逻辑成功和失败各自的回调函数：

```typescript
moduleContext.execute('getList', (data, extra, result) => {
  // 逻辑成功回调函数会传入返回数据、附加数据和未经处理的结果
}, (message, result) => {
  // 逻辑失败回调函数会传入错误提示和未经处理的结果
});
```

#### 请求函数

虽然在 Handie 内部服务端动作实际是一个像模块描述器一样的 JavaScript 对象，也就是动作描述器，但为了尽量符合开发者习惯，可以直接用异步函数来表示：

```typescript
import { ResponseResult } from 'handie-vue';

import httpClient from '@/utils/http';

import { AnimationEntity } from './typing';

// 获取列表数据
async function getList(condition: Record<string, any>): Promise<ResponseResult<AnimationEntity[]>> {
  return httpClient.get('/api/animations', { params: condition });
}

// 获取详情数据
async function getOne(id: string): Promise<ResponseResult<AnimationEntity>> {
  return httpClient.get(`/api/animations/${id}`);
}

export { getList, getOne };
```

定义好服务端动作之后，需要全部导出并赋值给模块描述器：

```typescript
import { ModuleDescriptor } from 'handie-vue';

import * as actions from './repository';

export default {
  name: 'animation',
  actions,
} as ModuleDescriptor;
```

需要注意：**模块描述器上的 `actions` 属性只能是服务端动作，不能是客户端动作！**

#### 命名规则

理论上 Handie 对被转换为服务端动作的异步函数的命名没有任何限制，但在视图上下文处理 CRUD 相关操作时会默认执行几个约定好的服务端动作：

| 上下文 | 动作名 | 说明 |
| --- | --- | --- |
| 列表类视图上下文 | `getList` | 获取带分页的列表数据 |
|  | `deleteOne` | 删除单条数据 |
|  | `deleteList` | 删除多条数据 |
| 对象类视图上下文 | `getOne` | 获取详情数据 |
|  | `insert` | 插入数据 |
|  | `update` | 更新数据 |


若在定义服务端动作时没使用上述命名，需在视图描述器中显式声明各个动作的实际名称：

```typescript
export default {
  name: 'AnimationListView',
  category: 'list',
  renderType: 'table',
  // 以下为显式声明 CRUD 相关动作名
  getList: 'getAnimationList',
  deleteOne: 'deleteAnimationById',
};
```

### 客户端动作

客户端动作就一定要用动作描述器的形式了，无法进行简化：

```typescript
{
  context: 'free',
  text: '新增',
  execute: (context: ViewContext, vm) =>
    vm.$router.push({ name: `${context.getModuleContext().getModuleName()}NewForm` }),
}
```

上面这段代码就是用几个必备的属性定义了一个常规的跳转到表单页的「新增」操作。

#### 关键的 `context`

动作描述器的 `context` 属性很重要，它决定了该动作在哪个位置渲染，在什么时候可操作以及在执行时传入的视图上下文是列表上下文还是对象上下文。

这个属性的值有 `single`、`batch`、`both` 和 `free`，若不显式指定，默认为 `single`。除了值为 `single` 的动作，其他的都只能用在列表类视图中。

值为 `single` 的动作的语义是「单条操作」，只要显示了就可操作；值是 `batch` 和 `both` 的动作的语义都是「批量操作」，不同点在于 `batch` 是严格意义上的批量操作，即只有选中多于一条记录时才可操作，而 `both` 则只要选中一条记录即可操作；值为 `free` 的动作正如其名，不受选中记录数量的限制，一直可操作。

在列表类视图中，当值为 `single` 时，默认会出现在条目的操作区，如表格的操作列，并且在执行时传入的是对象上下文；其他值的动作都是在条目的外部渲染，如表格的顶部，执行时传入的是列表上下文。

#### 二次确认

有些时候，需要在真正执行逻辑前进行二次确认提示，这时可以给动作描述器添加 `confirm` 属性来达到这个目的。可以将它的值设为 `true` 来使用默认的提示信息，也可自定义一段纯文本。

#### UX 语义

从 UX 角度来看，对于操作经常要从视觉上区分出主要操作、次要操作和危险操作，Handie 对此进行了一定程度的支持。

就按上面的示例代码来说，这是一个次要操作。如果要指明那个「新增」是一个主要操作，需给动作描述器加上 `primary` 属性并设为 `true`，这样一来就会在视觉上显示表示「主要」的主题色。

相较于主要操作，在视觉上感知到某个操作会导致不可逆的结果更为重要！为动作描述器加上 `danger` 属性并设为 `true`，就会看到表示「危险」的主题色并在点击时用默认的提示信息进行二次确认提示，想要自定义提示信息需结合 `confirm` 属性。

#### 动态展示

对单条数据的操作，其可见性常会受行记录或表单数据的字段值影响，动作描述器的 `available` 属性就是用来应对这一场景的，其值是上下文表达式字符串：

```typescript
{
  text: '查看详情',
  available: "$value.status === 1",
  execute: (context: ObjectViewContext, vm) =>
    vm.$router.push({
      name: `${context.getModuleContext().getModuleName()}Detail`,
      params: { id: context.getFieldValue('id') },
    })
}
```

「上下文表达式字符串」是一个计算结果为 [truthy](https://developer.mozilla.org/en-US/docs/Glossary/Truthy)、[falsy](https://developer.mozilla.org/en-US/docs/Glossary/Falsy) 的表达式，并注入了几个变量用于辅助判断：

| 变量 | 说明 |
| --- | --- |
| `$dataSource` | 原始值/数据源，在单条操作时是对象，其他操作时是数组 |
| `$value` | 当前值，在单条操作时是对象（当前记录），其他操作时是数组（选中记录） |


## 视图定义

在 Handie 中，「视图」可以理解为是列表、对象这类数据结构的载体，像表格、表单就都是「视图」，无论它们是直接显示在界面的主内容区，还是嵌在某个字段里，或是在弹出的对话框、抽屉中。

看过上文后，相信聪明的你已经想到，要定义视图就需要「视图描述器」。上面介绍过的客户端动作的动作描述器就是要用在视图描述器中，以列表类视图为例：

```typescript
import { ObjectViewContext, ViewDescriptor } from 'handie-vue';

export default {
  name: 'AnimationListView',
  category: 'list',
  renderType: 'table',
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
} as ViewDescriptor;
```

示例中的 `category` 属性指的是视图类别，与其所承载的数据结构形态有关，目前只有 `list` 和 `object` 两种——前者用于普通列表、表格这种列表类视图；而后者用于表单、详情这种对象类视图。

属性 `renderType` 是已注册的视图部件的别名，用来查找被渲染的视图部件。Handie 官方已经提供了 CRUD 常用的 `table`、`form` 和 `detail` 这三个视图部件。

若它们不满足实际需求，可以自定义项目级、部门级或公司级的视图部件并注册进系统，在使用时通过视图描述器的 `renderType` 属性指定；也可以自定义模块级的视图部件，这时不需要注册，而是直接用 ES Modules 引入，然后赋值给视图描述器的 `widget` 属性。**`renderType` 属性和 `widget` 属性不能同时存在。**

如你所想，`actions` 属性就是用来指定客户端动作的，这里不能是服务端动作。

### 视图字段

「字段」是对象数据结构中键所对应的值，根据使用场景可细分为「视图字段」和「模型字段」，需要用字段描述器定义。

与模型字段相比，视图字段会多一些与展示相关的属性，而模型字段则是纯数据的。这里着重说下视图字段，模型字段下文中再阐述。

视图字段的属性分为数据和展示这两类：

| 类别 | 属性 | 是否必要 | 说明 |
| --- | --- | --- | --- |
| 数据 | `name` | 必需 | 在对象数据结构中的键名 |
|  | `dataType` | 必需 | 值的数据类型，Handie 内置了几个常用类型，可自定义进行扩展 |
|  | `label` | 可选 | 字段的文本标签，在表格中是列名 |
|  | `required` | 可选 | 是否必填，值可以是布尔型也可以是上下文表达式字符串 |
|  | `readonly` | 可选 | 是否只读，值可以是布尔型也可以是上下文表达式字符串 |
| 展示 | `placeholder` | 可选 | 值为空时的文本占位符 |
|  | `hint` | 可选 | 字段的说明提示 |
|  | `available` | 可选 | 字段的动态展示，值为上下文表达式字符串 |
|  | `renderType` | 可选 | 要渲染的已注册字段部件别名 |
|  | `widget` | 可选 | 模块级或视图级的自定义字段部件 |


上面所列出的是通用的属性，根据数据类型 `dataType` 的不同还会额外多出几个可选属性：

| 数据类型 | 附加属性 | 说明 |
| --- | --- | --- |
| `int` 或 `float` | `max` | 最大值 |
|  | `min` | 最小值 |
| `string` 或 `text` | `max` | 最大长度 |
|  | `min` | 最小长度 |
|  | `pattern` | 模式匹配 |
| `enum` 或 `multi-enum` | `options` | 枚举选项列表或返回枚举选项列表的函数，可以是异步函数 |


其中，枚举选项的结构为：

| 属性 | 是否必要 | 说明 |
| --- | --- | --- |
| `name` | 必需 | 选项名 |
| `label` | 必需 | 选项文本标签 |
| `value` | 必需 | 选项值 |
| `hint` | 可选 | 选项的说明提示 |
| `available` | 可选 | 选项的动态展示，值为上下文表达式字符串 |


在了解了如何描述字段之后，就在视图描述器中进行配置吧：

```typescript
import { ObjectViewContext, ViewDescriptor } from 'handie-vue';

export default {
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
} as ViewDescriptor;
```

### 搜索

在常规的中后台应用中，可以认为搜索条件的集合是对象类视图，是特化的表单；代表搜索条件的过滤器就相当于是字段。因此，搜索描述器和过滤器描述器分别与视图描述器和字段描述器如出一辙：

```typescript
import { ObjectViewContext, ViewDescriptor } from 'handie-vue';

export default {
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
} as ViewDescriptor;
```

### 视图配置项

TBD

## 模型定义

TBD
