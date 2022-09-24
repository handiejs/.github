用来描述字段相关信息，可在[模型描述器](/descriptors/model/)或[视图描述器](/descriptors/view/)中使用，可用属性会根据使用场景而不同。

## 属性

### 数据类

用来做些数据层面的处理，如取值、赋值、校验等；即可用在模型描述器中，也能在视图描述器中使用：

| 属性 | 值类型/可选值 | 是否必要 | 说明 |
| --- | --- | --- | --- |
| `name` | `string` | 必需 | 在对象数据结构中的键名 |
| `dataType` | `string` | 必需 | 值的数据类型，内置了几个常用类型，可自定义进行扩展，详见下方 |
| `label` | `string` | 可选 | 字段的文本标签，在表格中是列名 |
| `required` | `boolean | string` | 可选 | 是否必填，值可以是布尔型也可以是上下文表达式字符串（表达式字符串仅在视图描述器中可用） |
| `readonly` | `boolean | string` | 可选 | 是否只读，值可以是布尔型也可以是上下文表达式字符串（表达式字符串仅在视图描述器中可用） |

### 展示类

用来控制展示层面的事情，只能用在视图描述器中：

| 属性 | 值类型/可选值 | 是否必要 | 说明 |
| --- | --- | --- | --- |
| `placeholder` | `string` | 可选 | 值为空时的文本占位符 |
| `hint` | `string` | 可选 | 字段的说明提示 |
| `available` | `string` | 可选 | 字段的动态展示，值为上下文表达式字符串 |
| `formatter` | `(value: any, contextValue: Record<string, any>) => any` | 可选 | 将字段值格式化为想要的形式 |
| `renderType` | `string` | 可选 | 要渲染的已注册字段部件别名，框架已内置了一些[常用部件](/api/) |
| `widget` | `string | Function` | 可选 | 模块级或视图级的自定义字段部件 |
| `config` | `Record<string, any>` | 可选 | 表格列与字段部件的配置项，详见下方 |
| `hidden` | `boolean` | 可选 | 是否不显示，只是不渲染，在做值逻辑处理时还是照常 |

## 数据类型

内置的数据类型如下：

| 数据类型 | 值类型/可选值 | 默认值 | 说明 |
| --- | --- | --- | --- |
| `boolean` | `boolean` | `false` |  |
| `integer` | `number` | `0` |  |
| `float` | `number` | `0` |  |
| `string` | `string` | `''` |  |
| `text` | `string` | `''` |  |
| `enum` | `number | string` | `''` |  |
| `multi-enum` | `number[] | string[]` | `[]` |  |
| `date` | `Date | string | number | (Date | string | number)[]` | `''` |  |

上面所列出的是通用的属性，根据数据类型 `dataType` 的不同还会额外多出几个可选属性：

| 数据类型 | 附加属性 | 说明 |
| --- | --- | --- |
| `integer` 或 `float` | `max` | 最大值 |
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

## 配置项

### 表格列

当处在表格视图中时，字段描述器的 `config`  属性也起到列配置的作用：

| 配置项 | 值类型/可选值 | 默认值 | 说明 |
| --- | --- | --- | --- |
| `width` | `number | string` | - | 列宽 |
| `align` | `'left' | 'center' | 'right'` | - | 文本对齐方式 |
| `showOverflowTooltip` | `boolean` |  | 是否在文本溢出时悬停显示工具提示 |

### 编辑态

当处在表单视图或行内编辑等可编辑状态时生效的配置项：

| 配置项 | 值类型/可选值 | 默认值 | 说明 |
| --- | --- | --- | --- |
| `showHintAsPlaceholder` | `boolean` |  | 是否将字段提示显示为文本占位符 |

### 字段部件

指定字段部件所特有的配置项。内置字段部件的请见[相关说明](/api/)。
