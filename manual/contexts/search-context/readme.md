这里列出的方法是搜索上下文所独有的，其他的请见[值上下文](/contexts/value-context/)。

## API

### `getFilters()`

获取过滤器描述器列表。

### `getFilterValue(filterName)`

获取指定过滤器的值。指定过滤器不存在时返回 `undefined`，否则返回过滤器的值。

### `setFilterValue(filterName, filterValue)`

设置指定过滤器的值，会触发 `filterChange` 事件，并将过滤器信息作为载荷。

若过滤器不存在或设置的值不符合过滤器的数据类型会设置失败。
