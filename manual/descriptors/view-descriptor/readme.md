## 属性

### 通用

| 属性 | 值类型/可选值 | 是否必要 | 说明 |
| --- | --- | --- | --- |
| `name` | `string` | 必需 |  |
| `fields` | `(ViewFieldDescriptor | string)[]` | 必需 |  |
| `category` | `'list' | 'object'` | 可选 |  |
| `renderType` | `string` | 可选 |  |
| `widget` | `string | ComponentCtor` | 可选 |  |
| `actions` | `(ActionDescriptor | string)[]` | 可选 |  |
| `actionsAuthority` | `string | (...args: any[]) => string` | 可选 |  |
| `config` | `Record<string, any>` | 可选 |  |
| `opener` | `ListViewContext | ObjectViewContext` | 可选 |  |
| `initialValue` | `any` | 可选 |  |

### 列表视图

| 属性 | 值类型/可选值 | 是否必要 | 说明 |
| --- | --- | --- | --- |
| `search` | `SearchDescriptor | ComponentCtor` | 可选 |  |
| `getList` | `string` | 可选 |  |
| `deleteOne` | `string` | 可选 |  |
| `deleteList` | `string` | 可选 |  |

### 对象视图

| 属性 | 值类型/可选值 | 是否必要 | 说明 |
| --- | --- | --- | --- |
| `parent` | `ListViewContext` | 可选 |  |
| `indexInParent` | `number` | 可选 |  |
| `validate` | `'immediate' | 'submit' | 'none'` | 可选 |  |
| `insert` | `string` | 可选 |  |
| `update` | `string` | 可选 |  |
| `getOne` | `string` | 可选 |  |
