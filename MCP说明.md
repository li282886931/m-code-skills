# MCP 说明 — mcd-mcp（麦当劳官方 MCP）

本项目全部 9 个 Skill 均构建于 **mcd-mcp** 之上。mcd-mcp 是麦当劳官方提供的 MCP（Model Context Protocol）服务，将麦当劳 App 的核心能力以标准化工具接口的形式暴露出来，供 AI 助手在对话中直接调用。

> 本项目**不修改、不代理、不重打包** mcd-mcp 的任何服务，仅编排其公开工具接口。使用前需在 WorkBuddy 中连接 mcd-mcp 连接器并完成麦当劳账号授权。

---

## 一、MCP 基本信息

| 项目 | 说明 |
|------|------|
| 名称 | `mcd-mcp`（麦当劳官方 MCP） |
| 协议 | Model Context Protocol (MCP) |
| 接入方式 | 在 WorkBuddy 连接器管理中添加 / 启用 mcd-mcp |
| 授权 | 需登录麦当劳账号（查询积分、订单、领券等需鉴权） |
| 工具数量 | 35 个（本项目实测） |
| 覆盖场景 | 门店、餐品、营养、优惠、价格、订单、积分、商城、抽奖、派对、评价、配送 |

---

## 二、能力总览

mcd-mcp 的能力可归为 **8 大类**：

```
mcd-mcp
├── 1. 门店与配送     —— 找店、选店、地址管理
├── 2. 餐品与营养     —— 菜单、餐品详情、营养成分、点餐辅助
├── 3. 优惠与价格     —— 领券、查券、比价、促销活动
├── 4. 订单管理       —— 下单、查单、取消
├── 5. 积分与商城     —— 积分账户、积分兑换、商城下单
├── 6. 抽奖          —— 抽奖信息、抽奖、中奖记录
├── 7. 派对预订       —— 城市/门店/日期/场次、派对下单
└── 8. 评价奖励       —— 满意度问卷奖励券
```

---

## 三、工具清单（35 个）

### 1. 门店与配送

| 工具 | 说明 |
|------|------|
| `query-nearby-stores` | 到店/得来速门店查询（支持位置搜索、收藏） |
| `delivery-query-stores` | 外送/团餐场景门店查询（需地址） |
| `delivery-query-addresses` | 查询已保存的配送地址 |
| `delivery-create-address` | 新增配送地址 |
| `now-time-info` | 服务器当前时间（预约场景确定日期） |

### 2. 餐品与营养

| 工具 | 说明 |
|------|------|
| `query-meals` | 门店餐品列表（分类、价格、标签、图片） |
| `query-meal-detail` | 单个餐品详情（含特调 modification 选项） |
| `list-nutrition-foods` | 全量餐品营养成分（能量/蛋白质/脂肪/碳水/钠/钙） |
| `query-meal-assistance` | 点餐辅助信息 |

### 3. 优惠与价格

| 工具 | 说明 |
|------|------|
| `auto-bind-coupons` | 一键自动领取当前可用优惠券 |
| `available-coupons` | 查询可领取但未领取的券 |
| `query-my-coupons` | 查询用户卡包中已有的券 |
| `query-store-coupons` | 查询指定门店当前可用的券 |
| `calculate-price` | 计算订单价格（支持券、随单购等优惠） |
| `query-promotions` | 查询满减/满折等促销活动 |
| `campaign-calendar` | 活动日历 |

### 4. 订单管理

| 工具 | 说明 |
|------|------|
| `create-order` | 创建订单 |
| `query-order` | 查询单个订单详情 |
| `order-list` | 查询历史订单列表 |
| `cancel-order` | 取消订单 |

### 5. 积分与商城

| 工具 | 说明 |
|------|------|
| `query-my-account` | 积分账户全景（可用/累计/已用/过期） |
| `mall-points-products` | 积分商城商品列表 |
| `mall-product-detail` | 商城商品详情 |
| `mall-create-order` | 商城兑换下单 |
| `mall-order-list` | 商城订单列表 |
| `mall-order-detail` | 商城订单详情 |

### 6. 抽奖

| 工具 | 说明 |
|------|------|
| `query-lottery-info` | 抽奖活动信息 |
| `draw-lottery` | 执行抽奖 |
| `query-my-prizes` | 查询我的中奖记录 |

### 7. 派对预订

| 工具 | 说明 |
|------|------|
| `query-party-city` | 查询开放派对预订的城市 |
| `query-party-store` | 查询城市内可预订派对的门店 |
| `query-party-store-date` | 查询门店可预约日期 |
| `query-party-store-session` | 查询门店可预约场次 |
| `party-order-create` | 创建派对预订订单 |

### 8. 评价奖励

| 工具 | 说明 |
|------|------|
| `query-survey-coupon` | 查询订单满意度问卷关联的奖励券 |

---

## 四、关键调用规则（踩坑备忘）

以下是本项目在实测中总结的 mcd-mcp 参数规则，已写入各 Skill 的 `references/tool_chain.md`：

### 场景类型 `beType` 与 `orderType`

| beType | 场景 | orderType | beCode |
|--------|------|-----------|--------|
| 1 | 到店自取 | 1 | 不传 |
| 5 | 得来速 | 1 | 必传 |
| 2 | 麦乐送 | 2 | 必传 |
| 6 | 团餐 | 2 | 必传 |

- `query-nearby-stores`：`searchType=2`（位置搜索）时必填 `city` + `keyword`。
- 预约场景：`query-meals` 需传 `reservationDate`（格式 `yyyy-MM-dd HH:mm`）。
- **价格单位为「分」**，展示时需 ÷100 转为「元」。
- 券的可用性需以 `query-store-coupons` 为准，`query-my-coupons` 的券不保证当前门店可用。
- 随单购商品（`canWithOrder=true`）需在 `calculate-price` 中传 `withOrder` 才享折扣。

---

## 五、Skill 与 MCP 工具的映射

| Skill | 使用的 mcd-mcp 工具 |
|-------|---------------------|
| mcd-nutrition-advisor | `list-nutrition-foods` `query-meals` `query-nearby-stores` |
| mcd-deal-hunter | `auto-bind-coupons` `available-coupons` `query-my-coupons` `query-store-coupons` `calculate-price` `query-promotions` |
| mcd-points-strategist | `query-my-account` `mall-points-products` `mall-product-detail` `query-lottery-info` `draw-lottery` `query-my-prizes` |
| mcd-party-planner | `query-party-city` `query-party-store` `query-party-store-date` `query-party-store-session` `party-order-create` |
| mcd-mood-menu | `query-meals` `list-nutrition-foods` `query-nearby-stores` |
| mcd-order-detective | `order-list` `query-order` `mall-order-list` `mall-order-detail` `query-meals` `list-nutrition-foods` |
| mcd-survey-rewards | `order-list` `query-survey-coupon` `query-order` |
| mcd-social-feast | `delivery-query-addresses` `delivery-create-address` `delivery-query-stores` `query-meals` `query-store-coupons` `auto-bind-coupons` `calculate-price` `query-meal-assistance` `create-order` |
| mcd-secret-menu | `query-meals` `query-meal-detail` `calculate-price` `create-order` |

---

## 六、免责声明

- mcd-mcp 由麦当劳官方提供，其接口、参数与返回字段可能随版本更新而变化；本项目文档基于实测记录，仅供参考。
- 本项目仅对 mcd-mcp 的公开工具进行编排，**不涉及任何账号凭据的存储或转发**。
- 涉及下单、兑换、抽奖等会产生实际消费或消耗积分的操作，均需用户明确确认后执行。
