用于开发其他结构部件：

```typescript
export default class AnotherStructuralWidget extends BaseStructuralWidget {
  ...
}
```

## 辅助方法

以下 API 是所有结构部件通用的——

### `$$view`

访问当前视图上下文。

### `$$module`

访问当前模块上下文。

### `$$app`

访问应用助手。

### `config`

访问当前部件通过相应描述器传入的 `config`。

### `setHeadlessWidget(hw)`

设置当前结构部件对应的无头部件以处理一些逻辑。

### `setBehaviors(keyInTheme, options)`

设置部件特有的全局默认行为配置。

### `getBehavior(path)`

获取部件特有的全局默认行为配置。

### `getCommonBehavior(path, defaultBehavior?)`

获取通用的全局默认行为配置。

### `setStyleClassNames(styleClassNames)`

设置样式类名映射。主要用于使用了 CSS Modules 的场景。

### `getStyleClassName(className)`

通过原始类名获取转换后的类名。

### `on(event, handler?)`

监听当前视图上下文的某个事件。通过这种方式添加的事件会在部件销毁时自动解除。

### `off(event?, handler?)`

停止监听当前视图上下文的某个事件。
