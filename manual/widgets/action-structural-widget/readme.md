用于开发动作部件：

```typescript
export default class YourActionWidget extends ActionStructuralWidget {
  ...
}
```

## 部件配置

通过动作描述器的 `config` 属性来配置。

| 配置项 | 值类型/可选值 | 默认值 | 说明 |
| --- | --- | --- | --- |
| `className` | `string` | - | 附加的 CSS 类名 |
| `disableWhenNoSelection` | `boolean` | 随主题 `common.action.disableWhenNoSelection` | 批量操作类动作是否随着列表选中条目数更改可用状态 |
| `view` | `string` | - | 对话框中要显示的视图，是 `模块名.views.视图名` 的格式 |

## 辅助方法

以下 API 是所有动作结构部件通用的——

### `resolveClassNames(className?)`

获取处理后的类名。将固有的 `ActionWidget`、传进来的 `className` 和通过 `config` 指定的 `className` 按顺序拼接。

### `renderView()`

渲染视图。

### `renderContent()`

渲染动作内容。

### `onExecute()`

点击动作后执行逻辑。当有视图传入时，会触发动作所在视图的自定义上下文事件 `dialog-view-show`；否则执行通过动作描述器生成的逻辑。
