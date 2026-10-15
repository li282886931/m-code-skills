# m-code-skills 🍔

基于 **mcd-mcp**（麦当劳官方 MCP）设计的一组创意 WorkBuddy Skill，把麦当劳 30+ 个原子化接口按**用户真实场景**重新编排为完整工作流，覆盖点餐、优惠、积分、派对、社交、隐藏玩法等全链路。

每个 Skill 都是**自包含**的包：包含 `SKILL.md`（主文档，含触发描述与工作流）+ `references/tool_chain.md`（mcd-mcp 工具链参数参考），可被 WorkBuddy 在匹配触发场景时自动加载。

---

## 目录

- [项目亮点](#项目亮点)
- [Skill 矩阵（9 个）](#skill-矩阵9-个)
- [项目结构](#项目结构)
- [安装](#安装)
- [使用示例](#使用示例)
- [MCP 说明](./MCP说明.md)
- [参赛申明](./参赛申明.md)
- [License](#license)

---

## 项目亮点

- **场景化编排**：不止是 API 封装，而是把零散接口组织成"省钱达人""派对策划师""订单侦探"等完整角色工作流。
- **全链路覆盖**：从查门店 → 选餐 → 领券 → 比价 → 下单 → 评价 → 积分兑换 → 派对预订，端到端打通。
- **创意玩法**：把心情、消费画像、社交聚餐、隐藏菜单等"人味"需求映射到 API 能力上。
- **规范合规**：全部 Skill 遵循 WorkBuddy 官方 Skill 规范，通过 `quick_validate.py` 校验。

---

## Skill 矩阵（9 个）

### 第一波 · 实用工具

| Skill | 定位 | 关键词 | 核心工具链 |
|-------|------|--------|-----------|
| **mcd-nutrition-advisor** | 营养搭配顾问 | 热量、卡路里、蛋白质、减脂餐、500大卡 | `list-nutrition-foods` `query-meals` `query-nearby-stores` |
| **mcd-deal-hunter** | 省钱达人 | 优惠、领券、满减、折扣、性价比 | `auto-bind-coupons` `available-coupons` `query-store-coupons` `calculate-price` `query-promotions` |
| **mcd-points-strategist** | 积分管家 | 麦享会、积分兑换、积分抽奖、积分过期 | `query-my-account` `mall-points-products` `mall-product-detail` `query-lottery-info` `draw-lottery` `query-my-prizes` |
| **mcd-party-planner** | 派对策划师 | 生日派对、主题派对、包场、拼团 | `query-party-city` `query-party-store` `query-party-store-date` `query-party-store-session` `party-order-create` |

### 第二波 · 创意玩法

| Skill | 定位 | 关键词 | 核心工具链 |
|-------|------|--------|-----------|
| **mcd-mood-menu** | 心情点餐师 | 心情、情绪、累、烦、开心、嘴馋、犒劳 | `query-meals` `list-nutrition-foods` `query-nearby-stores` |
| **mcd-order-detective** | 订单侦探 | 消费习惯、历史订单、口味画像、换个口味 | `order-list` `query-order` `mall-order-list` `query-meals` `list-nutrition-foods` |
| **mcd-survey-rewards** | 评价官 | 评价、满意度、问卷、评价奖励、评价券 | `order-list` `query-survey-coupon` |
| **mcd-social-feast** | 社交聚餐管家 | 聚餐、多人、朋友、团建、合单 | `delivery-query-addresses` `query-meals` `auto-bind-coupons` `calculate-price` `create-order` |
| **mcd-secret-menu** | 隐藏菜单探索 | 特调、加料、去酱、隐藏菜单、customization | `query-meals` `query-meal-detail` `calculate-price` `create-order` |

---

## 项目结构

```
m-code-skills/
├── README.md                    # 项目说明（本文件）
├── MCP说明.md                    # mcd-mcp 能力说明与工具清单
├── 参赛申明.md                   # 参赛声明
├── .gitignore
├── mcd-nutrition-advisor/
│   ├── SKILL.md                 # 主文档：触发描述 + 核心能力 + 工作流 + 重要规则
│   └── references/
│       └── tool_chain.md        # 工具链参考：mcd-mcp 工具参数、调用规则、返回字段
├── mcd-deal-hunter/
│   ├── SKILL.md
│   └── references/tool_chain.md
├── mcd-points-strategist/
├── mcd-party-planner/
├── mcd-mood-menu/
├── mcd-order-detective/
├── mcd-survey-rewards/
├── mcd-social-feast/
└── mcd-secret-menu/
```

每个 Skill 目录遵循 WorkBuddy 官方规范：

```
mcd-<skill-name>/
├── SKILL.md              # 主文档：触发描述 + 核心能力 + 工作流 + 重要规则
└── references/
    └── tool_chain.md     # 工具链参考：mcd-mcp 工具参数、调用规则、返回字段
```

---

## 安装

```bash
# 方式一：复制到 WorkBuddy skills 目录
cp -R mcd-* ~/.workbuddy-ai/skills/

# 方式二：软链接（便于随仓库更新）
ln -s "$(pwd)"/mcd-* ~/.workbuddy-ai/skills/
```

安装后需确保 WorkBuddy 已连接 **mcd-mcp** 连接器（麦当劳官方 MCP）。

---

## 使用示例

直接用自然语言触发即可，WorkBuddy 会按描述匹配最合适的 Skill：

| 你说 | 触发 Skill |
|------|-----------|
| "今天心情不好想吃点甜的" | `mcd-mood-menu` |
| "我想 500 大卡以内搞定一顿麦当劳" | `mcd-nutrition-advisor` |
| "帮我看看今晚吃什么最划算" | `mcd-deal-hunter` |
| "我还有多少积分？快过期了" | `mcd-points-strategist` |
| "想给娃办个麦当劳生日派对" | `mcd-party-planner` |
| "我平时都在麦当劳点啥" | `mcd-order-detective` |
| "我点过的单子有评价奖励没拿" | `mcd-survey-rewards` |
| "明天 3 个人一起点麦乐送" | `mcd-social-feast` |
| "巨无霸能不能去酸黄瓜加芝士" | `mcd-secret-menu` |

---

## 依赖

- WorkBuddy AI 客户端
- **mcd-mcp** 连接器（麦当劳官方 MCP）— 详见 [MCP说明.md](./MCP说明.md)
- Python 3.9+（仅在本地打包 / 校验 Skill 时使用，非运行时依赖）

---

## License

MIT
