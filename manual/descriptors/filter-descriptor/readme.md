## 属性

| 属性 | 值类型/可选值 | 是否必要 | 说明 |
| --- | --- | --- | --- |
| `name` | `string` | 必需 |  |
| `dataType` | `string` | 可选 |  |
| `label` | `string` | 可选 |  |
| `required` | `boolean | string` | 可选 |  |
| `defaultValue` | `boolean | string` | 可选 |  |
| `renderType` | `string` | 可选 | 要渲染的已注册过滤器部件别名，框架已内置了一些[常用部件](/api/) |
| `widget` | `string | Function` | 可选 | 模块级或视图级的自定义过滤器部件 |
| `config` | `Record<string, any>` | 可选 |  |
| `hidden` | `boolean` | 可选 | 是否不显示，只是不渲染，在做值逻辑处理时还是照常 |
