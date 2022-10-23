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

## 底层基础

### 描述器

| 描述器 | 说明 |
| --- | --- |
| [模块描述器](/descriptors/module-descriptor) | - |
| [模型描述器](/descriptors/model-descriptor) | - |
| [动作描述器](/descriptors/action-descriptor) | - |
| [视图描述器](/descriptors/view-descriptor) | - |
| [字段描述器](/descriptors/field-descriptor) | - |
| [搜索描述器](/descriptors/search-descriptor) | - |
| [过滤器描述器](/descriptors/filter-descriptor) | - |

### 上下文

| 上下文 | 说明 |
| --- | --- |
| [模块上下文](/contexts/module-context) | - |
| [值上下文](/contexts/value-context) | - |
| [搜索上下文](/contexts/search-context) | - |
| [视图上下文](/contexts/view-context) | - |

### 无头部件

| 部件 | 说明 |
| --- | --- |
| [基础无头部件](/widgets/base-headless-widget/) | - |
| [视图无头部件](/widgets/view-headless-widget/) | - |
| [字段无头部件](/widgets/field-headless-widget/) | - |
| [动作无头部件](/widgets/action-headless-widget/) | - |
| [过滤器无头部件](/widgets/filter-headless-widget/) | - |
| [布局无头部件](/widgets/layout-headless-widget/) | - |

### 其他

| 资源 | 说明 |
| --- | --- |
| [主题配置](/theme/) | - |
| [工具函数](/utils/) | - |

## 渲染适配

### 渲染器

| 渲染器 | 说明 |
| --- | --- |
| [视图渲染器](/renderers/view-renderer) | - |
| [表单渲染器](/renderers/form-renderer) | - |
| [字段渲染器](/renderers/field-renderer) | - |
| [动作渲染器](/renderers/action-renderer) | - |
| [过滤器渲染器](/renderers/filter-renderer) | - |

### 结构部件

封装了较为通用的逻辑与部分视图结构的半成品结构部件，可在此之上根据需求进行完整的视图结构渲染。

| 部件 | 说明 |
| --- | --- |
| [基础结构部件](/widgets/base-structural-widget/) | - |
| [字段结构部件](/widgets/field-structural-widget/) | - |
| [管理布局结构部件](/widgets/admin-layout-structural-widget/) | 用于中后台应用页面的整体布局 |

## 内置部件

为支持常规业务需求的快速开发，预设了一些承载使用频率较高的交互模式的功能完整的结构部件。

### 视图部件

| `category` 值 | `renderType` 值 | 部件 |
| --- | --- | --- |
| `list` | `table` | [表格视图部件](/widgets/table-view-widget/) |
| `object` | `form` | [表单视图部件](/widgets/form-view-widget/) |
|  | `detail` | [详情视图部件](/widgets/detail-view-widget/) |
|  | `form-dialog` | [表单对话框视图部件](/widgets/form-dialog-view-widget/) |

### 字段部件

| `dataType` 值 | `renderType` 值 | 部件 |
| --- | --- | --- |
| `boolean` | `switch` | [开关字段部件](/widgets/switch-field-widget/) |
|  | `radio` | [单选框字段部件](/widgets/radio-field-widget/) |
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

### 动作部件

| `renderType` 值 | 部件 |
| --- | --- |
| `button` | [按钮动作部件](/widgets/button-action-widget/) |
| `link` | [链接动作部件](/widgets/link-action-widget/) |
| `icon` | [图标动作部件](/widgets/icon-action-widget/) |
| `dialog-view-button` | [对话框视图按钮动作部件](/widgets/dialog-view-button-action-widget/) |

### 搜索部件

| `renderType` 值 | 部件 |
| --- | --- |
| `form` | [表单搜索部件](/widgets/form-search-widget/) |

### 过滤器部件

| `dataType` 值 | `renderType` 值 | 部件 |
| --- | --- | --- |
| `string` | `input` | [单行文本框过滤器部件](/widgets/input-filter-widget/) |
| `text` | `input` | [单行文本框过滤器部件](/widgets/input-filter-widget/) |
| `enum` | `select` | [下拉列表过滤器部件](/widgets/select-filter-widget/) |
| `multi-enum` | `select` | [下拉列表过滤器部件](/widgets/select-filter-widget/) |
| `date` | `date-range` | [日期范围过滤器部件](/widgets/date-range-filter-widget/) |
|  | `date-time-range` | [日期时间范围过滤器部件](/widgets/date-time-range-filter-widget/) |
