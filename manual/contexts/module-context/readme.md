## API

### `getModuleName()`

获取模块名，类型为 `string`。

### `getModel()`

获取与当前模块绑定的[模型描述器](/descriptors/model-descriptor/)，如果没有进行绑定就是 `undefined`。

### `getDependencies(refPath?)`

获取模块依赖。

返回目标依赖 map。如果不传 `refPath`，返回的是全部依赖资源；传 `[module-name]` 返回指定依赖模块的资源；若传 `[module-name].[resource-type]` 则返回指定模块特定类型的资源。找不到依赖资源时会返回 `undefined`。

### `getComponents(newComponents?)`

除了不用传 `moduleName`，其他与全局函数的 `getComponents` 一样。

### `execute(actionName, paramsOrSuccess?, successOrFail?, fail?)`

执行指定已绑定到模块上的服务端动作，返回被执行的服务端动作的返回值。
