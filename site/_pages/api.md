---
title: API 总览
layout: guide
css:
  - pages/post
  - pages/doc
js:
  - initializers/time
  - initializers/lazyload
repo: api
---

## 内置部件

### 视图部件

| `category` 值 | `renderType` 值 | 部件 |
| --- | --- | --- |
| `list` | `table` | [表格视图部件](/widgets/table-view-widget/) |
| `object` | `form` | [表单视图部件](/widgets/form-view-widget/) |
| `object` | `detail` | [详情视图部件](/widgets/detail-view-widget/) |
| `object` | `form-dialog` | [表单对话框视图部件](/widgets/form-dialog-view-widget/) |

### 字段部件

| `dataType` 值 | `renderType` 值 | 部件 |
| --- | --- | --- |
| `boolean` | `radio` | [单选框字段部件](/widgets/radio-field-widget/) |
|  | `switch` | [开关字段部件](/widgets/switch-field-widget/) |
| `integer` | `number` | [数字输入框字段部件](/widgets/number-field-widget/) |
| `float` | `number` | [数字输入框字段部件](/widgets/number-field-widget/) |
| `string` | `input` | [单行文本框字段部件](/widgets/input-field-widget/) |
|  | `password` | [密码输入框字段部件](/widgets/password-field-widget/) |
|  | `url` | [URL 输入框字段部件](/widgets/url-field-widget/) |
| `text` | `textarea` | [多行文本框字段部件](/widgets/textarea-field-widget/) |
| `enum` | `select` | [下拉列表字段部件](/widgets/select-field-widget/) |
|  | `radio` | [单选框字段部件](/widgets/radio-field-widget/) |
| `multi-enum` | `select` | [下拉列表字段部件](/widgets/select-field-widget/) |
| `date` | `date` | [日期字段部件](/widgets/date-field-widget/) |
|  | `date-range` | [日期范围字段部件](/widgets/date-range-field-widget/) |
|  | `date-time` | [日期时间字段部件](/widgets/date-time-field-widget/) |
|  | `date-time-range` | [日期时间范围字段部件](/widgets/date-time-range-field-widget/) |

### 过滤器部件

| `dataType` 值 | `renderType` 值 | 部件 |
| --- | --- | --- |
| `string` | `input` | [单行文本框过滤器部件](/widgets/input-filter-widget/) |
| `text` | `input` | [单行文本框过滤器部件](/widgets/input-filter-widget/) |
| `enum` | `select` | [下拉列表过滤器部件](/widgets/select-filter-widget/) |
| `multi-enum` | `select` | [下拉列表过滤器部件](/widgets/select-filter-widget/) |
| `date` | `date-range` | [日期范围过滤器部件](/widgets/date-range-filter-widget/) |
|  | `date-time-range` | [日期时间范围过滤器部件](/widgets/date-time-range-filter-widget/) |
