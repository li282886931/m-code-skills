# m-code-skills 🍔

基于 [mcd-mcp](https://www.workbuddy.cn) (麦当劳官方 MCP) 设计的一组创意 WorkBuddy Skill，覆盖麦当劳点餐、优惠、积分、派对、社交、隐藏玩法等全链路场景。

每个 Skill 都是**自包含**的包：包含 `SKILL.md`（主文档，含触发词与工作流）+ `references/tool_chain.md`（mcd-mcp 工具链参数参考），可被 WorkBuddy 在匹配触发场景时自动加载。

## Skill 矩阵（9 个）

### 第一波 · 实用工具

| Skill | 定位 | 关键词 |
|-------|------|--------|
| **mcd-nutrition-advisor** | 营养搭配顾问 | 热量、卡路里、蛋白质、减脂餐、500大卡 |
| **mcd-deal-hunter** | 省钱达人 | 优惠、领券、满减、折扣、性价比 |
| **mcd-points-strategist** | 积分管家 | 麦享会、积分兑换、积分抽奖、积分过期 |
| **mcd-party-planner** | 派对策划师 | 生日派对、主题派对、包场、拼团 |

### 第二波 · 创意玩法

| Skill | 定位 | 关键词 |
|-------|------|--------|
| **mcd-mood-menu** | 心情点餐师 | 心情、情绪、累、烦、开心、嘴馋、犒劳 |
| **mcd-order-detective** | 订单侦探 | 消费习惯、历史订单、口味画像、换个口味 |
| **mcd-survey-rewards** | 评价官 | 评价、满意度、问卷、评价奖励、评价券 |
| **mcd-social-feast** | 社交聚餐管家 | 聚餐、多人、朋友、团建、合单 |
| **mcd-secret-menu** | 隐藏菜单探索 | 特调、加料、去酱、隐藏菜单、customization |

## 安装

```bash
# 复制到你的 WorkBuddy skills 目录
cp -R mcd-* ~/.workbuddy-ai/skills/

# 或软链接
ln -s /path/to/m-code-skills/mcd-nutrition-advisor ~/.workbuddy-ai/skills/mcd-nutrition-advisor
```

## 使用

直接用自然语言触发即可，WorkBuddy 会按描述匹配最合适的 Skill：

- "今天心情不好想吃点甜的" → `mcd-mood-menu`
- "我想 500 大卡以内搞定一顿麦当劳" → `mcd-nutrition-advisor`
- "帮我看看今晚吃什么最划算" → `mcd-deal-hunter`
- "我还有多少积分？快过期了" → `mcd-points-strategist`
- "想给娃办个麦当劳生日派对" → `mcd-party-planner`
- "我平时都在麦当劳点啥" → `mcd-order-detective`
- "我点过的单子有评价奖励没拿" → `mcd-survey-rewards`
- "明天 3 个人一起点麦乐送" → `mcd-social-feast`
- "巨无霸能不能去酸黄瓜加芝士" → `mcd-secret-menu`

## Skill 结构

每个 Skill 目录遵循 WorkBuddy 官方规范：

```
mcd-<skill-name>/
├── SKILL.md              # 主文档：触发描述 + 核心能力 + 工作流 + 重要规则
└── references/
    └── tool_chain.md     # 工具链参考：mcd-mcp 工具参数、调用规则、返回字段
```

## 依赖

- WorkBuddy AI 客户端
- mcd-mcp 连接器（麦当劳官方 MCP）
- Python 3.9+（仅在本地打包时使用，非运行时依赖）

## License

MIT
