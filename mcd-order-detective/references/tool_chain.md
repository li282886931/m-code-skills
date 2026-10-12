# Mcd-mcp 工具链参考 — 订单分析相关

## 餐品订单

### order-list — 历史订单列表
- 无参数
- 返回: 用户的麦当劳历史订单(非商城订单)
- 商城订单需用 `mall-order-list` 单独查询

### query-order — 订单详情
- 必填: `orderId`(订单号)
- 返回: 单笔订单的完整信息(餐品明细、金额、时间、状态)

### cancel-order — 取消订单
- 必填: `orderId`, `cancelReasonCode`
- cancelReasonCode 可选值: 1=改主意了, 2=重复下单, 3=点错了/点多了/点少了, 4=地址/电话填错了, 5=送达时间选错了, -1=其它原因

## 商城订单

### mall-order-list — 商城订单列表
- 可选: `lastId`(分页游标), `size`(默认10, 最大10)

### mall-order-detail — 商城订单详情
- 必填: `orderId`

## 餐品与营养

### query-meals — 餐品列表
- 用于 "换个口味" 推荐时获取全量菜单
- 必填: `storeCode`, `orderType`, `beType`

### list-nutrition-foods — 营养成分
- 用于"换个口味"推荐的营养对比

## 数据聚合分析模式

```
order-list → 获取订单列表
    ↓
对每笔订单 query-order(orderId) → 获取详情
    ↓
聚合数据 → 总消费/订单数/平均客单价/常点餐品 Top 5
    ↓
交叉分析 → 口味标签/搭配习惯/价格偏好/探索指数
    ↓
query-meals + list-nutrition-foods → "换个口味"推荐
```

## 重要提醒

- 订单数据需逐条 query-order 才能看到餐品明细
- 样本量不足(<3笔)时提示"数据量较少，画像仅供参考"
- 商城订单和餐品订单需分别查询
