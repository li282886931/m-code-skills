# Mcd-mcp 工具链参考 — 营养分析相关

## 门店查询

### query-nearby-stores — 到店场景门店查询
- 必填: `beType`(1=到店自取, 5=得来速), `searchType`(1=收藏, 2=位置搜索)
- searchType=2 时必填: `city`, `keyword`
- 返回: storeCode, storeName, address, distance, businessStatus, reservation, reservationTimeOptions
- beType=1 时无 beCode; beType=5 时有 beCode, 后续调用需传入

### delivery-query-stores — 外送场景门店查询
- 必填: `beType`(2=麦乐送, 6=团餐), `addressId`(来自 delivery-query-address)
- 返回: storeCode, beCode

## 餐品查询

### query-meals — 餐品列表
- 必填: `storeCode`, `orderType`, `beType`
- orderType 与 beCode 规则:
  - beType=1 → orderType=1, 不传 beCode
  - beType=5 → orderType=1, 必传 beCode
  - beType=2 → orderType=2, 必传 beCode
  - beType=6 → orderType=2, 必传 beCode
- 预约场景必传: `reservationDate`(格式 yyyy-MM-dd HH:mm)
- 返回: categories(分类列表), meals(餐品详情映射, 含 currentPrice/originalPrice/image/tags)
- 餐品标签 tags 含: 上新, 爆款, 麦金卡, 套餐, 单品, 限定特惠 等

### list-nutrition-foods — 营养成分数据
- 无参数
- 返回: 全量餐品的营养成分(能量/蛋白质/脂肪/碳水化合物/钠/钙等)

## 当前时间

### now-time-info — 服务器时间
- 无参数
- 用于预约场景确定当前日期
