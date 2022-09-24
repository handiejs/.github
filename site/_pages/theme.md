---
title: 主题配置
layout: guide
css:
  - pages/post
  - pages/doc
js:
  - initializers/time
  - initializers/lazyload
repo: api
---

## 图标

| 配置项 | 值类型/可选值 | 默认值 | 说明 |
| --- | --- | --- | --- |
| `providers` | `{ [key: string]: IconProvider }` | - | 详见 Petals 中 `Icon` 的关于[定制「图标提供者」](https://petals.fxxk.design/controls/icon/)的相关描述 |

## 行为

### 动作

| 配置项 | 值类型/可选值 | 默认值 | 说明 |
| --- | --- | --- | --- |
| `renderType` | `'button' | 'link' | 'icon'` | `'button'` |  |
| `showIcon` | `boolean` | `false` | 是否显示图标 |
| `iconOnly` | `boolean` | `false` | 是否只显示图标 |
| `disableWhenNoSelection` | `boolean` | `true` |  |

### 过滤器

| 配置项 | 值类型/可选值 | 默认值 | 说明 |
| --- | --- | --- | --- |
| `dateFilterRenderType` | `'date' | 'date-range' | 'date-time' | 'date-time-range'` | `'date-range'` |  |
| `dateTimeFormat` | `string` | `'YYYY-MM-DD HH:mm:ss'` |  |
| `dateFormat` | `string` | `'YYYY-MM-DD'` |  |
| `showHintAsPlaceholder` | `boolean` | `true` | 输入提示作为过滤器占位符显示 |
| `showValidationRulesAsNative` | `boolean` | `false` | 校验规则作为原生属性 |
| `showEmptyValueOption` | `boolean` | `true` | 是否显示空值（值为空字符串）的选项 |
| `emptyValueOptionLabel` | `string` | `'全部'` | 空值选项显示的文本 |

### 搜索

| 配置项 | 值类型/可选值 | 默认值 | 说明 |
| --- | --- | --- | --- |
| `conditionPersists` | `boolean` | `false` |  |
| `searchWhenSelectableFilterChange` | `boolean` | `false` |  |

### 字段

| 配置项 | 值类型/可选值 | 默认值 | 说明 |
| --- | --- | --- | --- |
| `booleanFieldRenderType` | `'select' | 'radio' | 'checkbox' | 'switch'` | `'radio'` |  |
| `enumFieldRenderType` | `'select' | 'radio'` | `'radio'` |  |
| `dateFieldRenderType` | `'date' | 'date-range' | 'date-time' | 'date-time-range'` | `'date'` |  |
| `dateTimeFormat` | `string` | `'YYYY-MM-DD HH:mm:ss'` |  |
| `dateFormat` | `string` | `'YYYY-MM-DD'` |  |
| `showUnavailableOption` | `boolean` | `false` |  |
| `showHintAsPlaceholder` | `boolean` | `true` | 输入提示作为表单控件占位符显示 |
| `showHintAtFormItem` | `boolean` | `false` | 输入提示显示在表单条目中 |
| `hintPositionOfFormItem` | `'explain' | 'label'` | `'explain'` | 表单条目中输入提示所在位置 |
| `hintIcon` | `string` | `'question'` |  |
| `showValidationRulesAsNative` | `boolean` | `false` | 校验规则作为原生属性 |

### 视图

| 配置项 | 值类型/可选值 | 默认值 | 说明 |
| --- | --- | --- | --- |
| `objectViewFormLayout` | `'horizontal' | 'vertical' | 'inline'` | `'horizontal'` | 表单布局 |
| `objectViewFormControlLabelWidth` | `number | string` | `200` | 表单控件文本标签宽度 |
| `objectViewFormControlSize` | `'large' | 'medium' | 'small'` | `'medium'` |  |
| `objectViewShowValidationMessage` | `boolean` | `true` |  |
| `objectViewActionBarOutside` | `boolean` | `false` |  |
| `objectViewActionBarAlignment` | `'left' | 'center' | 'right'` | `'left'` |  |
| `listViewDensity` | `'high' | 'medium' | 'low'` | `'medium'` |  |
| `listViewPageSizes` | `number[]` | `[10, 20, 50, 100]` |  |
| `listViewDefaultPageSize` | `number` | `20` |  |
