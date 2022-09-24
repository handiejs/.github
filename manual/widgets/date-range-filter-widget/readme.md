适用于数据类型为 `date` 的过滤器。

## 部件配置

通过过滤器描述器的 `config` 属性来配置。

| 配置项 | 值类型/可选值 | 默认值 | 说明 |
| --- | --- | --- | --- |
| `searchImmediately` | `boolean` |  |  |
| `format` | `string` |  |  |
| `valueFormat` | `string` | 随 `format` |  |
| `showNow` | `boolean` | - |  |
| `disableDate` | `(value: DateValue[], date: Date, contextValue: ObjectValue) => boolean` | - |  |
| `separator` | `string` | `'-'` |  |
| `fromField` | `string` | - |  |
| `fromPlaceholder` | `string` | - |  |
| `toField` | `string` | - |  |
| `toPlaceholder` | `string` | - |  |
