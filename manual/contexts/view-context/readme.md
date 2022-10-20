这里列出的方法是视图上下文所独有的，其他的请见[值上下文](/contexts/value-context/)。

## 通用

### `getModuleContext()`

获取所属模块的[模块上下文](/contexts/module-context/)。

### `getComponents()`

获取所属模块在模块描述器中声明的用到的 UI 组件。

### `getId()`

获取随机生成的视图上下文 ID。

### `getOpener()`

### `getView()`

获取[视图描述器](/descriptors/view-descriptor/)。

### `getFields()`

获取[视图字段描述器](/descriptors/field-descriptor/)列表。

### `getActions()`

获取客户端动作列表。

### `getActionsByContextType(contextType)`

按动作上下文类型获取客户端动作列表。

### `getActionsAuthority()`

获取总控的动作权限。

### `getConfig()`

获取用于视图部件渲染的配置。

### `getDataSource()`

获取数据源。列表视图上下文返回的是数组，对象视图上下文返回对象。

### `setDataSource(data)`

设置数据源。列表视图上下文需要传入数组，对象视图上下文传入对象。会触发 `dataChange` 事件，并将数据源作为载荷。

### `getBusy()`

获取忙碌状态。

### `setBusy(busy)`

设置忙碌状态。会触发 `busyChange` 事件，并将忙碌状态作为载荷。

### `load()`

加载数据。一般用于正常流程的数据加载。

### `reload()`

重载数据。一般用于「刷新」数据。

## 列表

列表视图上下文独有。

### `getChildren()`

获取子视图上下文列表。

### `getSearch()`

获取搜索描述。若没设置会返回 `undefined`，否则返回搜索描述器或搜索部件。

### `getSearchContext()`

获取[搜索上下文](/contexts/search-context/)。

### `getTotal()`

获取分页总数。

### `getCurrentPage()`

获取当前页数。

### `setCurrentPage(current)`

设置当前页数后重载数据。会触发 `currentPageChange` 事件，并将当前页数作为载荷。

### `getPageSize()`

获取当前每页条数。

### `setPageSize(size)`

设置当前每页条数后重载数据。会触发 `pageSizeChange` 事件，并将当前每页条数作为载荷。

### `getList(params, success?, fail?)`

获取列表数据的请求，是相应的服务端动作的简便方法。返回被执行的服务端动作的返回值。

### `deleteOne(params, success?, fail?)`

删除单条数据的请求，是相应的服务端动作的简便方法。返回被执行的服务端动作的返回值。

### `deleteList(params, success?, fail?)`

删除多条数据的请求，是相应的服务端动作的简便方法。返回被执行的服务端动作的返回值。

## 对象

对象视图上下文独有。

### `getParent()`

### `getIndexInParent()`

### `getFieldValue(fieldName)`

获取指定字段的值。指定字段不存在时返回 `undefined`，否则返回字段的值。

### `setFieldValue(fieldName, fieldValue)`

设置指定字段的值，会触发 `fieldChange` 事件，并将字段信息作为载荷。

若字段不存在或设置的值不符合字段的数据类型会设置失败。

### `setFieldChecker(fieldName, valueChecker)`

### `isModified()`

当前值是否更新过。通过 `setValue()` 和 `setFieldValue()` 设置过值就算更新过，在 `submit()` 和 `reset()` 后会重置状态。

### `getOne(id, success?, fail?)`

获取单条数据的请求，是相应的服务端动作的简便方法。返回被执行的服务端动作的返回值。

### `insert(data, success?, fail?)`

插入数据的请求，是相应的服务端动作的简便方法。返回被执行的服务端动作的返回值。

### `update(data, success?, fail?)`

更新数据的请求，是相应的服务端动作的简便方法。返回被执行的服务端动作的返回值。
