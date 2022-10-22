除了通用的过滤器无头部件之外，还根据内置的数据类型而细分出了多个过滤器无头部件，这里只罗列出有自己特有 API 的过滤器无头部件。

## 通用

### `getPlaceholder()`

获取占位文本。

### `isValidationRulesShownAsNative()`

是否将校验规则作为[宿主环境的原生规则](https://developer.mozilla.org/en-US/docs/Learn/Forms/Form_validation)。

## 枚举过滤器

### `initOptions(viewContext, callback)`

初始化枚举过滤器的选项。

## 日期过滤器

虽然底层使用 [Day.js](https://day.js.org/) 进行日期相关的格式转换与计算，但其[日期格式](https://day.js.org/docs/en/parse/string-format)与 [Moment.js](https://momentjs.com/) 是通用的，故而在自定义部件或基于第三方 UI 组件库开发日期相关部件时只要日期格式通用就行，不限制是用 Day.js、Moment.js 或是其他的什么。

### `setDefaultFormat(format)`

设置默认的日期格式。

### `getDisplayFormat()`

获取显示时的日期格式。

### `resolveDateValue(date)`

获取转换后的日期值。

### `getRangeValue()`

获取格式化后用于展示的日期/时间范围值。

### `getRangePlaceholders()`

获取日期/时间范围过滤器起始时间与结束时间的占位文本。

### `getSeparator()`

获取日期/时间范围过滤器起始时间与结束时间之间展示用的分隔符。默认为连字符 `-`。
