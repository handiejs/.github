[视图上下文](/contexts/view-context/)和[搜索上下文](/contexts/search-context/)都属于值上下文，所以值上下文中的方法与它们通用。

## API

### `on(eventNameOrHandlers, handler?)`

添加事件监听器。

### `off(eventName?, handler?)`

移除事件监听器。

### `emit(eventName, payload?)`

触发指定事件。

### `getDefaultValue()`

获取默认值。返回创建上下文时传入的 `defaultValue`。

### `getValue()`

获取当前值。返回上下文中维护的当前活动值。

### `setValue(value)`

设置当前值，会触发 `change` 事件，并将当前值作为载荷。

### `isReady()`

### `submit()`

提交当前值，会触发 `submit` 事件。

### `reset()`

重置当前值为默认值，会触发 `reset` 事件，并将当前值作为载荷。
