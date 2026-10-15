# 麦当劳 MCP 集成说明

本仓库是基于麦当劳中国提供的 [麦当劳 MCP Server](https://github.com/M-China/mcd-mcp-server) 构建的 9 个 WorkBuddy Skill，属于参赛者的场景化编排项目，**不是麦当劳官方产品**。项目没有自建或代理 MCP 服务，不包含真实令牌。

## Server 与接入

- Server：麦当劳中国 MCP Server，接入地址 `https://mcp.mcd.cn`。
- 协议：Streamable HTTP；认证为 `Authorization: Bearer <MCP Token>`，令牌由使用者在麦当劳开放平台申请。
- WorkBuddy：在自定义连接器中配置、启用 `mcd-mcp`，再按自然语言触发本仓库中的 Skill。
- 脱敏配置：见 [`mcp-config.example.json`](./mcp-config.example.json)。其中 `${MCD_MCP_TOKEN}` 是**环境变量占位符，不是真实令牌**；如果客户端不支持环境变量展开，请依官方连接指南在本地安全配置，不要将实际令牌写入仓库或公开对话。
- 官方文档：[MCP Server 使用指南](https://github.com/M-China/mcd-mcp-server)；接口、字段和价格均以实时服务为准。

## 实际工具调用与工作流

本项目已在 WorkBuddy 对话中实际调用过 `now-time-info`、`query-my-account`、`query-nearby-stores`、`query-meals`，用于验证时间、积分账户、门店和菜单查询链路。其余下列编排依据连接器提供的工具定义和各 Skill 文档设计，**不表示已逐项完成端到端实测**。

| Skill | MCP Tool（工作流中调用） | 调用流程与业务价值 |
|---|---|---|
| `mcd-nutrition-advisor` | `list-nutrition-foods`、`query-nearby-stores`、`query-meals` | 获取营养数据→检索门店在售菜单→在热量和蛋白质目标下推荐真实可售餐品。 |
| `mcd-deal-hunter` | `available-coupons`、`query-store-coupons`、`calculate-price` | 检索可用优惠→校验当前门店与商品适用性→逐方案算价，呈现更省钱的选择。 |
| `mcd-points-strategist` | `query-my-account`、`mall-points-products`、`mall-product-detail` | 查询积分及到期情况→浏览商城→权衡兑换方案；抽奖工具仅在明确确认后使用。 |
| `mcd-party-planner` | `query-party-city`、`query-party-store`、`query-party-store-date`、`query-party-store-session` | 按城市→门店→日期→场次逐步缩小范围；最后经用户确认才创建派对订单。 |
| `mcd-mood-menu` | `query-meals`、`list-nutrition-foods` | 基于用户心情与饮食偏好，从门店当前在售商品中生成场景化推荐。 |
| `mcd-order-detective` | `order-list`、`query-order`、`query-meals` | 对当前用户授权范围内的订单做口味画像，再结合当前菜单推荐新品。 |
| `mcd-survey-rewards` | `order-list`、`query-survey-coupon` | 筛选已完成订单→查询其问卷奖励状态，避免遗漏可领权益。 |
| `mcd-social-feast` | `delivery-query-addresses`、`delivery-query-stores`、`query-meals`、`calculate-price` | 查询配送条件→收集多人偏好→算价和分摊；仅确认后创建订单。 |
| `mcd-secret-menu` | `query-meals`、`query-meal-detail`、`calculate-price` | 检查某一餐品的**实际可用**特制/换购选项并估算价格，不承诺未返回的选项。 |

各 Skill 的 `references/tool_chain.md` 记录所需参数、调用顺序和注意事项；以 API 最新定义和实际返回为准。

## 典型对话流程

1. 用户说：“帮我在国贸附近选一份相对低热量的套餐。”
2. 助手调用 `query-nearby-stores`，让用户确定候选门店（此前曾实测“北京·国贸”搜索场景）。
3. 调用 `query-meals` 获取该店当前在售菜单；调用 `list-nutrition-foods` 获取营养成分。
4. 将菜单餐品与营养条目对应，说明匹配程度、热量和可能的缺失数据；提供多个备选方案。
5. 若涉及下单，展示最终价格与订单明细，**待用户明确确认**后再使用 `create-order`；信息仍以服务实时返回为准。

## 安全与边界

- 对话和仓库都不包含个人积分、历史订单、地址、手机号、支付信息或真实 MCP Token；隐私数据只在用户授权的当前会话内查询。
- 营养建议是信息辅助，非医疗建议；价格、库存、券可用性需实时核验。
- 下单、积分兑换、抽奖、派对预订等有实际代价的操作，须先向用户说明内容及费用并获明确确认。
