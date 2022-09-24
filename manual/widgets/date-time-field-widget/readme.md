适用于数据类型为 `date` 的字段。

## 部件配置

通过字段描述器的 `config` 属性来配置。

| 配置项 | 值类型/可选值 | 默认值 | 说明 |
| --- | --- | --- | --- |
| `format` | `string` |  |  |
| `valueFormat` | `string` | 随 `format` |  |
| `showNow` | `boolean` | - |  |
| `disableDate` | `(value: DateValue, date: Date, contextValue: ObjectValue) => boolean` | - |  |
