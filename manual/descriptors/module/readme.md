## 属性

| 属性 | 值类型/可选值 | 是否必要 | 说明 |
| --- | --- | --- | --- |
| `name` | `string` | 必需 | 模块名字 |
| `model` | `ModelDescriptor` | 可选 |  |
| `actions` | `Record<string, ServerAction &#124; AsyncFunction>` | 可选 |  |
| `views` | `Record<string, ViewComponentRenderer>` | 可选 |  |
| `imports` | `string[]` | 可选 |  |
| `exports` | `Partial<Record<ModuleResourceType, Record<string, any>>>` | 可选 |  |
| `components` | `Record<string, boolean &#124; string>` | 可选 |  |

