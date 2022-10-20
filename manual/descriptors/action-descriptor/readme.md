## 属性

### 通用

| 属性 | 值类型/可选值 | 是否必要 | 说明 |
| --- | --- | --- | --- |
| `name` | `string` | 必需 |  |
| `category` | `'server' | 'client'` | 必需 |  |

### 客户端动作

| 属性 | 值类型/可选值 | 是否必要 | 说明 |
| --- | --- | --- | --- |
| `context` | `'free' | 'single' | 'batch' | 'both'` | 可选 |  |
| `authority` | `string | (...args: any[]) => string` | 可选 |  |
| `text` | `string` | 可选 |  |
| `primary` | `boolean` | 可选 |  |
| `danger` | `boolean` | 可选 |  |
| `confirm` | `boolean | string` | 可选 |  |
| `renderType` | `string` | 可选 |  |
| `widget` | `string | ComponentCtor` | 可选 |  |
| `config` | `Record<string, any>` | 可选 |  |
| `available` | `string` | 可选 |  |
| `execute` | `<VC>(viewContext: VC, app: AppHelper, config: Record<string, any>) => Promise<any> | any` | 可选 |  |

### 服务端动作

| 属性 | 值类型/可选值 | 是否必要 | 说明 |
| --- | --- | --- | --- |
| `execute` | `<RT, PT>(params?: PT) => Promise<RT>` | 必需 |  |
