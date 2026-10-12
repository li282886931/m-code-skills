# Mcd-mcp 工具链参考 — 心情点餐相关

## 餐品与菜单

### query-meals — 餐品列表
- 必填: `storeCode`, `orderType`, `beType`
- beCode 规则: beType=1 不传, beType=5/2/6 必传
- 预约场景必传: `reservationDate`(格式 yyyy-MM-dd HH:mm)
- 返回 categories(分类列表) 和 meals 映射(餐品详情)
- **关键字段 tags**: 餐品标签列表，是语义匹配的核心
  - 口味类: 浓郁奶香拉丝芝士, 韩式辣椒黄油风味, 蓝莓, 灰焰, 上新
  - 类别: 套餐, 单品, 小食, 冰淇淋, 派, 冷饮, 热饮
  - 卖点: 爆款, 人气经典, 新升级, 心形薯饼同款比心
  - 优惠: 麦金卡, 限定特惠, 立省10.5元起
- **关键字段 currentPrice/originalPrice**: 现价/原价，用于价格展示
- **关键字段 discountType**: 享受的优惠类型(null, 促销优惠, 麦金卡优惠, 早餐卡优惠, 随单购麦金卡优惠, 随单购早餐卡优惠)

### list-nutrition-foods — 营养成分数据
- 无参数
- 用于补充营养信息(低卡/高蛋白筛选)
- 与 query-meals 的餐品名交叉匹配

## 门店查询

### query-nearby-stores — 到店场景
- 必填: `beType`(1=到店自取, 5=得来速), `searchType`(1=收藏, 2=位置搜索)
- searchType=2 时必填: `city`, `keyword`
- 用于获取 storeCode

## 重要提醒

- 推荐必须基于 `query-meals` 返回的在售菜单，不推荐已下架餐品
- 情绪映射时优先用 tags 做语义匹配（如"浓郁"匹配含"浓郁"tag的餐品）
- 情感化推荐理由是核心，每个推荐必须附"为什么适合心情"
