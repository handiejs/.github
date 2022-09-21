## 部件配置

通过动作描述器的 `config` 属性来配置。

| 配置项 | 值类型/可选值 | 默认值 | 说明 |
| --- | --- | --- | --- |
| `className` | `string` | - | 附加的 CSS 类名 |
| `showIcon` | `boolean` | 随主题 `common.action.showIcon` | 是否显示图标 |
| `icon` | `string` | - | 详见 Petals 中 `Icon` [API](https://petals.fxxk.design/controls/icon/) 的 `refs` 属性 |
| `iconOnly` | `boolean` | 随主题 `common.action.iconOnly` | 是否仅显示图标，当 `showIcon` 为 `true` 且指定了 `icon` 时生效 |
| `disableWhenNoSelection` | `boolean` | 随主题 `common.action.disableWhenNoSelection` | 批量操作类动作是否随着列表选中条目数更改可用状态 |
| `size` | `'large' | 'medium' | 'small'` | - | 按钮尺寸 |
| `view` | `string` | - | 对话框中要显示的视图，是 `模块名.views.视图名` 的格式 |
