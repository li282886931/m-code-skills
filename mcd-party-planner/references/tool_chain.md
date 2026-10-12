# Mcd-mcp 工具链参考 — 派对预订相关

## 派对工具调用链(严格按序)

```
mall-points-products (选活动)
    ↓ 获取 spuId
mall-product-detail (查看详情)
    ↓ 确认 partyType, shopId, skuId
query-party-city (选城市)
    ↓ 获取 city code, latitude, longitude
query-party-store (选门店)
    ↓ 获取 storeCode
query-party-store-date (选日期)
    ↓ 获取 dateStr
query-party-store-session (选场次)
    ↓ 获取 id, timeStart, timeEnd, leftNum
party-order-create (创建订单)
    ↓ 完成
```

## 工具详情

### mall-points-products — 查询派对类商品
- 传 catRuleIds 筛选派对类型:
  - 1>6>20: 生日类派对
  - 1>6>21: 主题类派对
  - 1>6>22: 麦麦体验营
  - 1>6>25: 品鉴会
  - 1>6>34: 读书会
  - 1>6>40: 积分兑换活动

### mall-product-detail — 商品详情
- 必填: `spuId`
- 返回: SKU 列表, partyType(1=包场, 2=拼团, -1=需询问), shopId(需=5才能下单)

### query-party-city — 查询城市
- 必填: `spuId`
- 返回: 城市 code, latitude, longitude, 城市名

### query-party-store — 查询门店
- 必填: `code`(城市code, 来自 query-party-city)
- 可选: `latitude`, `longitude`(来自 query-party-city)
- 可选: `spuId`
- 返回: storeCode, 门店名称, 地址

### query-party-store-date — 查询可预约日期
- 必填: `spuId`, `storeCode`
- 返回: 日期列表(dateStr)

### query-party-store-session — 查询场次
- 必填: `spuId`, `storeCode`, `dateStr`
- 返回: 场次列表, 含 id, timeStart, timeEnd, leftNum, partyMin, partyMax, price

### party-order-create — 创建派对订单
- 必填: `partyType`(1=包场, 2=拼团)
- 其他参数: spuId, skuId, code, storeCode, dateStr, id, timeStart, timeEnd, leftNum, count
- 仅支持 shopId=5 的商品
- count 不能超过 leftNum
- partyType=-1 时必须询问用户
