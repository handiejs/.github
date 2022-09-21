用于自定义页面的整体布局：

```typescript
export default class AdminLayoutWidget extends AdminLayoutStructuralWidget {
  ...
}
```

## 主题配置

通过应用主题的 `layout.admin` 来配置：

```typescript
createApp({
  theme: {
    behavior: {
      layout: {
        admin: {
          // 下表中的配置项
        }
      }
    }
  }
});
```

| 配置项 | 值类型/可选值 | 默认值 | 说明 |
| --- | --- | --- | --- |
| `navMenuPosition` | `'header' | 'aside' | 'both'` | `'both'` | 导航菜单的显示位置 |


## 辅助方法

### `getMenuFlags(): { active: string; open: string[] }`

获取当前访问地址所对应的该被激活的菜单项标识（`active`）及其祖先菜单项标识（`open`）。

### `renderNavMenu(renderNavLink: (text: string, location: string) => NodeType): NodeType`

生成导航菜单，根据 `navMenuPosition` 的不同，生成的结构会有差别——值为 `aside` 时生成纵向的导航菜单；而值为 `header` 和 `both` 时则生成横向的导航菜单，区别是在值为 `both` 时只会生成一级菜单，即路由配置的第一层。

当 `navMenuPosition` 的值为 `aside` 时应放置在侧边栏中，否则要放置在页头中。

### `renderSubNav(renderNavLink: (text: string, location: string) => NodeType): NodeType`

生成副导航菜单，只在 `navMenuPosition` 的值为 `both` 时有效。将路由配置中从第二层开始的嵌套路由生成多级的纵向导航菜单，应放置在侧边栏中。
