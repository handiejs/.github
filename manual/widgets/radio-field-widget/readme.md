适用于数据类型为 `boolean`、`enum` 的字段。

## 部件配置

通过字段描述器的 `config` 属性来配置。

### `boolean` 字段

| 配置项 | 值类型/可选值 | 默认值 | 说明 |
| --- | --- | --- | --- |
| `positiveLabel` | `string` | `'是'` | 正向文本标签 |
| `negativeLabel` | `string` | `'否'` | 负向文本标签 |
| `negativeFirst` | `boolean` | `false` | 是否负向选项在前 |

### `enum` 字段

| 配置项 | 值类型/可选值 | 默认值 | 说明 |
| --- | --- | --- | --- |
| `showUnavailableOption` | `boolean` |  | 是否显示不可用选项（以禁用状态展示） |
| `hintIcon` | `string` |  | 选项提示信息的图标，详见 Petals 中 `Icon` [API](https://petals.fxxk.design/controls/icon/) 的 `refs` 属性 |
