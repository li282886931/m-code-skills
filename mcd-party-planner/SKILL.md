---
name: mcd-party-planner
description: 麦当劳派对活动策划与预订顾问。当用户想了解或预订麦当劳的生日派对、主题派对、麦麦体验营、品鉴会、读书会、积分兑换活动，或需要查询派对城市、门店、可预约日期、场次信息并完成预订时，触发此技能。关键词：麦当劳派对、生日派对、主题派对、麦麦体验营、品鉴会、读书会、包场、拼团、party、birthday party、event booking。
agent_created: true
---

# Mcd Party Planner — 麦当劳派对策划师

## Overview

帮助用户从零到一完成麦当劳派对活动的策划与预订。覆盖活动类型选择、城市门店查询、日期场次选择、包场/拼团决策、最终下单全流程。支持生日类派对、主题类派对、麦麦体验营、品鉴会、读书会、积分兑换活动六大类型。

## Core Capabilities

### 1. 活动类型浏览

调用 `mall-points-products` 按派对类目筛选，展示各类活动：
- `1>6>20`：生日类派对 — 适合儿童生日庆祝
- `1>6>21`：主题类派对 — 主题定制活动
- `1>6>22`：麦麦体验营 — 互动体验活动
- `1>6>25`：品鉴会 — 美食品鉴活动
- `1>6>34`：读书会 — 阅读分享活动
- `1>6>40`：积分兑换活动 — 积分兑换参与

展示每种活动的内容描述、所需积分/价格、适合人数。

用户选定活动后，调用 `mall-product-detail` 获取 SKU 详情，确认规格、价格、partyType 等信息。

### 2. 城市与门店查询

**城市查询**：调用 `query-party-city`，传入 spuId，返回支持该活动的城市列表。展示城市名称、经纬度。

**门店查询**：用户选定城市后，调用 `query-party-store`，传入城市 code、经纬度、spuId，返回该城市支持该活动的门店列表。展示门店名称、地址、距离。

### 3. 日期与场次选择

**日期查询**：用户选定门店后，调用 `query-party-store-date`，传入 spuId 和 storeCode，返回可预约日期列表。

**场次查询**：用户选定日期后，调用 `query-party-store-session`，传入 spuId、storeCode、dateStr，返回当天可用场次。展示：
- 场次开始/结束时间
- 余位
- 最小/最大成团人数
- 价格

### 4. 包场 vs 拼团决策

根据 `mall-product-detail` 返回的 partyType 字段：
- `partyType=1`：仅支持包场
- `partyType=2`：仅支持拼团
- `partyType=-1`：需询问用户选择

**包场**：整场独享，适合生日派对、团建
**拼团**：与他人共享场次，价格更低，适合体验类活动

### 5. 完成预订

确认所有信息后，调用 `party-order-create` 创建派对订单，需要传入：
- spuId, skuId（从商品详情获取）
- partyType（包场/拼团）
- code（城市code）
- storeCode（门店code）
- dateStr（日期）
- id, timeStart, timeEnd, leftNum（场次信息）
- count（参与人数）

## Workflow

```
用户想办派对/活动
    │
    ├─ Step 1: 浏览活动类型
    │    └─ mall-points-products (catRuleIds=1>6>20/21/22/25/34/40)
    │    └─ mall-product-detail → 确认 SKU 与 partyType
    │
    ├─ Step 2: 选择城市
    │    └─ query-party-city (spuId) → 展示城市列表
    │
    ├─ Step 3: 选择门店
    │    └─ query-party-store (code, lat, lng, spuId) → 展示门店列表
    │
    ├─ Step 4: 选择日期
    │    └─ query-party-store-date (spuId, storeCode) → 展示可预约日期
    │
    ├─ Step 5: 选择场次
    │    └─ query-party-store-session (spuId, storeCode, dateStr) → 展示场次
    │
    ├─ Step 6: 确认参与人数与派对类型
    │    └─ 询问用户：包场还是拼团？几人参加？
    │
    └─ Step 7: 创建订单
         └─ party-order-create → 展示订单确认信息
```

## Important Rules

- 整个流程为严格的步骤式流程，每一步完成后必须等待用户明确选择，不得跳步
- `query-party-city` 和 `query-party-store` 必须传入 spuId，spuId 来自 `mall-points-products` 或 `mall-product-detail`
- `party-order-create` 仅支持 shopId=5 的商品，需在 `mall-product-detail` 确认
- 场次的 leftNum 表示剩余名额，参与人数(count)不能超过 leftNum
- partyType=-1 时必须询问用户，不可自行假设
- 预订信息确认时需完整展示：活动名称、城市、门店、日期、场次时间、参与人数、派对类型、总价/积分消耗
- 创建订单后引导用户："确认信息，可以说'创建订单'进行下单"
