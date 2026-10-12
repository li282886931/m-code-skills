# Mcd-mcp 工具链参考 — 省钱优惠相关

## 优惠券

### auto-bind-coupons — 一键领券
- 无参数, 自动领取所有可用券

### available-coupons — 查看可领券
- 无参数, 返回可领取但尚未领取的券列表

### query-my-coupons — 用户卡包券
- 可选: `page`(默认1, 最多5), `pageSize`(默认200, 最大200)
- 返回用户拥有的券, 不保证可用于当前门店/订单

### query-store-coupons — 门店可用券
- 必填: `storeCode`, `orderType`, `beType`
- beCode 规则: beType=1 不传, beType=5/2/6 必传
- 预约场景必传: `reservationDate`(格式 yyyy-MM-dd HH:mm)
- 返回当前门店+订单类型下可用的券

## 价格计算

### calculate-price — 计算商品价格
- 必填: `storeCode`, `orderType`, `beType`
- beCode 规则: beType=1 不传, beType=5/2/6 必传
- `items`: 商品列表, 每项含 productCode(必填), quantity(必填>=1), couponId/couponCode(选填)
- `withOrder`: 随单购商品, 含 membershipCode/specId/cardId/cardType
- `needTableware`: 是否需要餐具费
- 返回价格单位为"分", 展示时除以100转为"元"

## 促销查询

### query-promotions — 团餐促销规则
- 必填: `storeCode`, `orderType`(固定2), `beType`(固定6)
- 必传: `beCode`(从 delivery-query-stores 获取)
- 预约场景必传: `reservationDate`
- promotionType: 31=满额减, 33=订单折扣
- ruleCategory: 30=满折/折扣, 40=满减

## 餐品与门店

### query-meals — 餐品列表
- 必填: `storeCode`, `orderType`, `beType`
- 返回 currentPrice/originalPrice, canWithOrder, withOrder 等优惠相关字段

### query-nearby-stores / delivery-query-stores
- 获取 storeCode 和 beCode, 用于后续调用
