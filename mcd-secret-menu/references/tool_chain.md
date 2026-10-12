# Mcd-mcp 工具链参考 — 隐藏菜单相关

## 餐品与特调

### query-meals — 餐品列表
- 必填: `storeCode`, `orderType`, `beType`
- 返回: categories(分类) + meals 映射
- 用于扫描所有可特调餐品

### query-meal-detail — 餐品详情(含特调选项)
- 必填: `storeCode`, `orderType`, `beType`, `code`(餐品code)
- **关键字段 supportModify**: 是否可特调(true/false)
- **关键字段 modification**: 特调选项详情, 包含:
  - selectedKey: 选中项的 key
  - unselectedKey: 未选中项的 key(不为空时也必须传入)
  - choices[]: 特调选项列表
- **展示规则**:
  - supportModify=true 时在商品名称后标注【可特调】
  - 不要主动展开特调选项, 仅当用户询问"查看特调"时展示
  - 展示完详情后总结当前默认选中项(isDefault=1 的 choices)

## 特调参数组装规则

```
modification.values[]:
  - code: 特制商品 code
  - key: 特制 key
  - quantity: 特制数量

key 传参规则(必须严格遵守):
- 用户选中的特调项: key = selectedKey
- 用户未选中的特调项: 若 unselectedKey 不为空, key = unselectedKey(也必须传入)
- 即: 对于包含 unselectedKey 的特调组, 该组内所有特调项都必须传入
```

## 套餐子项更换

套餐子项的更换不通过 modification, 而是通过 `roundList` 参数:
- round: 套餐 round(如"饮料"、"小食")
- comboItemList: round 下的商品列表

## 价格计算与下单

### calculate-price — 价格计算
- items 中需传入 modification 完整参数
- 验证特调后价格变化

### create-order — 创建订单
- items 中需传入 modification
- takeWayCode 来自 calculate-price 的 takeWayList

## 重要提醒

- 特调可能改变价格, 必须 calculate-price 确认后再 create-order
- 不可主动展开特调选项, 等待用户询问
- 套餐换饮料/换小食用 roundList, 不是 modification
- supportModify=false 的餐品不能特调
- 隐藏搭配仅为推荐, 实际可用特调项以 query-meal-detail 返回为准
