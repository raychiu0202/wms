# WMS系统API接口文档

## 📋 接口基础信息

- **基础URL**: `http://localhost:8080`
- **接口前缀**: `/wms`
- **认证方式**: Bearer Token (Sa-Token)
- **请求格式**: JSON
- **响应格式**: JSON
- **在线文档**: `http://localhost:8080/doc.html`

### 认证头
```
Authorization: Bearer {token}
```

### 响应状态码
- `200`: 成功
- `401`: 未认证
- `403`: 无权限
- `500`: 服务器错误

---

## 🔐 认证接口

### 1. 用户登录
```http
POST /auth/login
Content-Type: application/json

{
  "username": "admin",
  "password": "admin123",
  "code": "1234",
  "uuid": "xxx-xxx-xxx"
}
```

**响应示例**：
```json
{
  "code": 200,
  "msg": "操作成功",
  "data": {
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "userInfo": {
      "userId": 1,
      "userName": "admin",
      "nickName": "管理员"
    }
  }
}
```

### 2. 获取验证码
```http
GET /auth/captchaImage
```

**响应示例**：
```json
{
  "code": 200,
  "msg": "操作成功",
  "data": {
    "uuid": "xxx-xxx-xxx",
    "img": "data:image/png;base64,iVBORw0KGgo..."
  }
}
```

### 3. 获取用户信息
```http
GET /system/user/getInfo
Authorization: Bearer {token}
```

**响应示例**：
```json
{
  "code": 200,
  "msg": "操作成功",
  "data": {
    "user": {
      "userId": 1,
      "userName": "admin",
      "nickName": "管理员",
      "email": "admin@ruoyi.com",
      "phonenumber": "13800138000"
    },
    "roles": ["admin"],
    "permissions": ["*:*:*"]
  }
}
```

---

## 📦 入库管理接口

### 1. 查询入库单列表
```http
GET /wms/receiptOrder/list
Authorization: Bearer {token}

?pageNum=1&pageSize=10&orderStatus=0
```

**请求参数**：
| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| pageNum | Integer | 否 | 页码 |
| pageSize | Integer | 否 | 每页数量 |
| orderNo | String | 否 | 入库单号 |
| orderStatus | Integer | 否 | 入库状态 |
| merchantId | Long | 否 | 商户ID |

**响应示例**：
```json
{
  "code": 200,
  "msg": "查询成功",
  "rows": [
    {
      "id": 1,
      "orderNo": "RK202404080001",
      "optType": 1,
      "merchantName": "供应商A",
      "orderStatus": 1,
      "totalQty": 100,
      "totalAmount": 5000.00,
      "createTime": "2024-04-08 10:00:00"
    }
  ],
  "total": 100
}
```

### 2. 获取入库单详情
```http
GET /wms/receiptOrder/{id}
Authorization: Bearer {token}
```

**响应示例**：
```json
{
  "code": 200,
  "msg": "操作成功",
  "data": {
    "id": 1,
    "orderNo": "RK202404080001",
    "optType": 1,
    "merchantId": 1,
    "merchantName": "供应商A",
    "orderStatus": 1,
    "totalQty": 100,
    "totalAmount": 5000.00,
    "details": [
      {
        "id": 1,
        "orderId": 1,
        "skuCode": "SKU001",
        "skuName": "商品A",
        "quantity": 50,
        "price": 50.00,
        "amount": 2500.00
      }
    ]
  }
}
```

### 3. 新增入库单
```http
POST /wms/receiptOrder
Authorization: Bearer {token}
Content-Type: application/json

{
  "optType": 1,
  "merchantId": 1,
  "details": [
    {
      "skuId": 1,
      "quantity": 100,
      "price": 50.00
    }
  ]
}
```

### 4. 修改入库单
```http
PUT /wms/receiptOrder
Authorization: Bearer {token}
Content-Type: application/json

{
  "id": 1,
  "optType": 1,
  "merchantId": 1,
  "orderStatus": 1,
  "details": [...]
}
```

### 5. 删除入库单
```http
DELETE /wms/receiptOrder/{ids}
Authorization: Bearer {token}
```

### 6. 查询入库单明细
```http
GET /wms/receiptOrder/detail/list
Authorization: Bearer {token}

?orderId=1
```

---

## 📤 出库管理接口

### 1. 查询出库单列表
```http
GET /wms/shipmentOrder/list
Authorization: Bearer {token}

?pageNum=1&pageSize=10&orderStatus=0
```

**请求参数**：
| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| pageNum | Integer | 否 | 页码 |
| pageSize | Integer | 否 | 每页数量 |
| orderNo | String | 否 | 出库单号 |
| orderStatus | Integer | 否 | 出库状态 |
| merchantId | Long | 否 | 商户ID |

### 2. 获取出库单详情
```http
GET /wms/shipmentOrder/{id}
Authorization: Bearer {token}
```

### 3. 新增出库单
```http
POST /wms/shipmentOrder
Authorization: Bearer {token}
Content-Type: application/json

{
  "optType": 4,
  "merchantId": 1,
  "details": [
    {
      "skuId": 1,
      "quantity": 50
    }
  ]
}
```

### 4. 修改出库单
```http
PUT /wms/shipmentOrder
Authorization: Bearer {token}
Content-Type: application/json

{
  "id": 1,
  "optType": 4,
  "merchantId": 1,
  "orderStatus": 1,
  "details": [...]
}
```

### 5. 删除出库单
```http
DELETE /wms/shipmentOrder/{ids}
Authorization: Bearer {token}
```

---

## 📊 库存管理接口

### 1. 查询库存列表
```http
GET /wms/inventory/list
Authorization: Bearer {token}

?pageNum=1&pageSize=10&warehouseId=1
```

**请求参数**：
| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| pageNum | Integer | 否 | 页码 |
| pageSize | Integer | 否 | 每页数量 |
| warehouseId | Long | 否 | 仓库ID |
| skuCode | String | 否 | SKU编码 |
| skuName | String | 否 | SKU名称 |

**响应示例**：
```json
{
  "code": 200,
  "msg": "查询成功",
  "rows": [
    {
      "id": 1,
      "warehouseId": 1,
      "warehouseName": "主仓库",
      "skuId": 1,
      "skuCode": "SKU001",
      "skuName": "商品A",
      "quantity": 1000,
      "lockQuantity": 50,
      "availableQuantity": 950
    }
  ],
  "total": 100
}
```

### 2. 库存统计
```http
GET /wms/inventory/statistic
Authorization: Bearer {token}
```

**响应示例**：
```json
{
  "code": 200,
  "msg": "操作成功",
  "data": {
    "totalItems": 500,
    "totalQuantity": 100000,
    "totalAmount": 5000000.00,
    "warehouseStats": [
      {
        "warehouseId": 1,
        "warehouseName": "主仓库",
        "quantity": 80000,
        "amount": 4000000.00
      }
    ]
  }
}
```

### 3. 查询库存历史
```http
GET /wms/inventoryHistory/list
Authorization: Bearer {token}

?pageNum=1&pageSize=10&operationType=1
```

**请求参数**：
| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| pageNum | Integer | 否 | 页码 |
| pageSize | Integer | 否 | 每页数量 |
| warehouseId | Long | 否 | 仓库ID |
| skuId | Long | 否 | SKU ID |
| operationType | Integer | 否 | 操作类型 |

**操作类型说明**：
- 1: 入库
- 2: 出库
- 3: 移库
- 4: 盘点

### 4. 导出库存历史
```http
POST /wms/inventoryHistory/export
Authorization: Bearer {token}
Content-Type: application/json

{
  "warehouseId": 1,
  "skuId": 1,
  "operationType": 1
}
```

---

## 🏢 基础数据接口

### 1. 仓库管理

#### 查询仓库列表
```http
GET /wms/warehouse/list
Authorization: Bearer {token}
```

#### 新增仓库
```http
POST /wms/warehouse
Authorization: Bearer {token}
Content-Type: application/json

{
  "warehouseName": "主仓库",
  "warehouseCode": "WH001",
  "address": "北京市朝阳区",
  "status": "0"
}
```

#### 修改仓库
```http
PUT /wms/warehouse
Authorization: Bearer {token}
Content-Type: application/json

{
  "id": 1,
  "warehouseName": "主仓库",
  "warehouseCode": "WH001",
  "address": "北京市朝阳区",
  "status": "0"
}
```

#### 删除仓库
```http
DELETE /wms/warehouse/{ids}
Authorization: Bearer {token}
```

### 2. 商户管理

#### 查询商户列表
```http
GET /wms/merchant/list
Authorization: Bearer {token}
```

#### 新增商户
```http
POST /wms/merchant
Authorization: Bearer {token}
Content-Type: application/json

{
  "merchantName": "供应商A",
  "merchantCode": "M001",
  "contactPerson": "张三",
  "phone": "13800138000",
  "email": "zhangsan@example.com",
  "address": "北京市海淀区"
}
```

### 3. 物料管理

#### 查询物料列表
```http
GET /wms/item/list
Authorization: Bearer {token}

?pageNum=1&pageSize=10&itemName=商品
```

#### 新增物料
```http
POST /wms/item
Authorization: Bearer {token}
Content-Type: application/json

{
  "itemName": "商品A",
  "itemCode": "ITEM001",
  "categoryId": 1,
  "brandId": 1,
  "unit": "件",
  "shelfLife": 365
}
```

### 4. 物料分类管理

#### 查询物料分类列表
```http
GET /wms/itemCategory/list
Authorization: Bearer {token}
```

#### 新增物料分类
```http
POST /wms/itemCategory
Authorization: Bearer {token}
Content-Type: application/json

{
  "categoryName": "电子产品",
  "categoryCode": "ELEC",
  "parentId": 0,
  "orderNum": 1
}
```

### 5. 物料品牌管理

#### 查询物料品牌列表
```http
GET /wms/itemBrand/list
Authorization: Bearer {token}
```

#### 新增物料品牌
```http
POST /wms/itemBrand
Authorization: Bearer {token}
Content-Type: application/json

{
  "brandName": "苹果",
  "brandCode": "APPLE"
}
```

### 6. 物料SKU管理

#### 查询物料SKU列表
```http
GET /wms/itemSku/list
Authorization: Bearer {token}

?pageNum=1&pageSize=10&itemId=1
```

#### 新增物料SKU
```http
POST /wms/itemSku
Authorization: Bearer {token}
Content-Type: application/json

{
  "itemId": 1,
  "skuCode": "SKU001",
  "spec": "红色/L码",
  "unit": "件",
  "price": 99.00
}
```

---

## 🔄 移库管理接口

### 1. 查询移库单列表
```http
GET /wms/movementOrder/list
Authorization: Bearer {token}

?pageNum=1&pageSize=10
```

### 2. 获取移库单详情
```http
GET /wms/movementOrder/{id}
Authorization: Bearer {token}
```

### 3. 新增移库单
```http
POST /wms/movementOrder
Authorization: Bearer {token}
Content-Type: application/json

{
  "fromWarehouse": 1,
  "toWarehouse": 2,
  "details": [
    {
      "skuId": 1,
      "quantity": 100
    }
  ]
}
```

### 4. 修改移库单
```http
PUT /wms/movementOrder
Authorization: Bearer {token}
Content-Type: application/json

{
  "id": 1,
  "fromWarehouse": 1,
  "toWarehouse": 2,
  "orderStatus": 1,
  "details": [...]
}
```

### 5. 删除移库单
```http
DELETE /wms/movementOrder/{ids}
Authorization: Bearer {token}
```

---

## 📋 盘点管理接口

### 1. 查询盘点单列表
```http
GET /wms/checkOrder/list
Authorization: Bearer {token}

?pageNum=1&pageSize=10&warehouseId=1
```

### 2. 获取盘点单详情
```http
GET /wms/checkOrder/{id}
Authorization: Bearer {token}
```

### 3. 新增盘点单
```http
POST /wms/checkOrder
Authorization: Bearer {token}
Content-Type: application/json

{
  "warehouseId": 1,
  "details": [
    {
      "skuId": 1,
      "systemQty": 100,
      "checkQty": 95
    }
  ]
}
```

### 4. 修改盘点单
```http
PUT /wms/checkOrder
Authorization: Bearer {token}
Content-Type: application/json

{
  "id": 1,
  "warehouseId": 1,
  "orderStatus": 1,
  "details": [...]
}
```

### 5. 完成盘点
```http
POST /wms/checkOrder/{id}/complete
Authorization: Bearer {token}
```

### 6. 删除盘点单
```http
DELETE /wms/checkOrder/{ids}
Authorization: Bearer {token}
```

---

## 📝 接口响应说明

### 通用响应格式
```json
{
  "code": 200,
  "msg": "操作成功",
  "data": {}
}
```

### 分页响应格式
```json
{
  "code": 200,
  "msg": "查询成功",
  "rows": [],
  "total": 100
}
```

### 错误响应格式
```json
{
  "code": 500,
  "msg": "系统错误",
  "detailMessage": "详细错误信息"
}
```

### 状态码说明
| 状态码 | 说明 |
|--------|------|
| 200 | 操作成功 |
| 401 | 未认证，请先登录 |
| 403 | 无权限访问 |
| 500 | 服务器内部错误 |

---

## 🔐 权限说明

### 权限标识格式
```
模块:功能:操作
```

### 常用权限标识
- `wms:receipt:list` - 查询入库单列表
- `wms:receipt:add` - 新增入库单
- `wms:receipt:edit` - 修改入库单
- `wms:receipt:remove` - 删除入库单
- `wms:shipment:list` - 查询出库单列表
- `wms:shipment:add` - 新增出库单
- `wms:shipment:edit` - 修改出库单
- `wms:shipment:remove` - 删除出库单
- `wms:inventory:list` - 查询库存列表
- `wms:inventory:query` - 查询库存详情
- `wms:movement:list` - 查询移库单列表
- `wms:movement:add` - 新增移库单
- `wms:check:list` - 查询盘点单列表
- `wms:check:add` - 新增盘点单
- `wms:warehouse:list` - 查询仓库列表
- `wms:merchant:list` - 查询商户列表
- `wms:item:list` - 查询物料列表

---

## 📚 注意事项

1. **认证要求**：除了登录接口，所有接口都需要在请求头中携带有效的token
2. **数据格式**：请求和响应数据格式为JSON，Content-Type需设置为application/json
3. **分页参数**：列表查询接口支持分页，默认每页10条记录
4. **错误处理**：调用接口时需要正确处理各种错误情况
5. **权限检查**：确保用户具有相应的权限标识

---

## 🔗 相关链接

- **在线API文档**: http://localhost:8080/doc.html
- **前端系统**: http://localhost:81/
- **后端服务**: http://localhost:8080/

---

*最后更新时间：2026-04-08*
