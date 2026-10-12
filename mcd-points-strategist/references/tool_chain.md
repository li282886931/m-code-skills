# Mcd-mcp 工具链参考 — 积分管理相关

## 积分账户

### query-my-account — 积分账户详情
- 无参数
- 返回字段(均为字符串类型):
  - availablePoint: 可用积分
  - accumulativePoint: 累计积分
  - usedPoint: 已使用积分
  - expiredPoint: 已过期积分
  - currentMouthExpirePoint: 本月将过期积分
  - nextMouthExpirePoint: 下月将过期积分
  - frozenPoint: 冻结积分

## 积分商城

### mall-points-products — 可兑换商品列表
- 可选: `catRuleIds`(类目筛选, 逗号分隔)
  - 1>4: 商品券
  - 2: 实物商品
  - 2>8: 周边产品
  - 2>9: 实物礼品卡
  - 1>6>20: 生日类派对
  - 1>6>21: 主题类派对
  - 1>6>22: 麦麦体验营
  - 1>6>25: 品鉴会
  - 1>6>34: 读书会
  - 1>6>40: 积分兑换活动

### mall-product-detail — 商品详情
- 必填: `spuId`(从 mall-points-products 获取)
- 返回: SKU 规格信息, 用户需选择具体 SKU 后才能兑换

## 积分抽奖

### query-lottery-info — 抽奖活动信息
- 无参数
- 返回: activityCode, activityName, activityStatusText, beginTime, endTime
  - drawPoint: 单次所需积分
  - drawTypeText: 抽奖消耗规则
  - availableTimes: 剩余可抽次数
  - availablePoint: 当前可用积分
  - drawDecision: { resourceEligible, nextConsumption, fallbackConsumption }
  - prizes: 奖品列表

### draw-lottery — 执行抽奖
- 无参数
- 前置条件: 必须先展示 drawDecision.nextConsumption 并获得用户明确确认
- resourceEligible=false 时禁止调用
- 短时间重复调用会返回"操作太频繁"
- 返回: status, win(是否中奖), prizes(中奖奖品)

## 奖品查询

### query-my-prizes — 抽奖奖品记录
- 可选: `pageNum`(默认1), `pageSize`(默认10, 最大50)
- 返回: prizes[{id, name, imageUrl, prizeType, recordTime, status, statusText, timeRemindText}]
- 按 recordTime 倒序
