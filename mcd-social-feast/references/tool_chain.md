# Mcd-mcp 工具链参考 — 多人聚餐相关

## 地址管理

### delivery-query-addresses — 查询配送地址
- 无参数
- 返回用户已有地址列表

### delivery-create-address — 创建配送地址
- 必填: `city`, `contactName`, `phone`(11位1开头), `address`, `addressDetail`
- 可选: `gender`(先生/女士)

### delivery-query-stores — 查询可配送门店
- 必填: `beType`(2=麦乐送, 6=团餐), `addressId`
- 返回: storeCode, beCode

## 餐品与价格

### query-meals — 餐品列表
- 必填: `storeCode`, `orderType`, `beType`
- beCode 规则: orderType=1+beType=1 不传; 否则必传

### query-store-coupons — 门店可用券
- 必填: `storeCode`, `orderType`, `beType`

### auto-bind-coupons — 一键领券
- 无参数, 减少人工遍历成本

### calculate-price — 价格计算
- 必填: `storeCode`, `orderType`, `beType`
- 多人下单时, items 合并为单一列表(quantity叠加)
- 价格单位为"分", 展示时除以100

## 助餐服务(团餐)

### query-meal-assistance — 助餐服务
- 必填: `storeCode`
- beType=6 场景专用
- gmServiceCode 传给 calculate-price

## 下单

### create-order — 创建订单
- 必填: `storeCode`, `orderType`, `beType`, `items`
- orderType=2 时必填 `addressId`
- orderType=1 时必填 `takeWayCode`(来自 calculate-price 的 takeWayList)

## 多人订单组装模式

```
1. 收集多人偏好 → 汇总商品清单
2. 合并 items(相同 productCode 累加 quantity, 不同 productCode 分项)
3. create-order 一次性下单, 配送到共享地址
4. 单独计算分摊明细(在 Skill 内部用本地逻辑, 不调用 MCP)
```

## 重要提醒

- 团餐场景(beType=6)需 query-meal-assistance 获取 gmServiceCode
- 人数>6 时建议用 beType=6 团餐场景
- 分摊计算需精确到分(共享项均分+个人项独立+配送费均分+优惠按比例分摊)
- 配送费按 calculate-price 实际返回值
