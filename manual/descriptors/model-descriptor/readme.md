用来定义模块的模型，字段信息可被所属模块下的任何视图所共用。

## 属性

| 属性 | 值类型/可选值 | 是否必要 | 说明 |
| --- | --- | --- | --- |
| `name` | `string` | 必需 | 模型名字，可与模块名相同 |
| `fields` | `FieldDescriptor[]` | 必需 | [字段描述器](/descriptors/field-descriptor) |
