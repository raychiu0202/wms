# WMS 仓库管理系统 - 项目文档

## 📋 项目概述

WMS (Warehouse Management System) 是基于 Spring Boot + Vue 3 开发的现代化仓库管理系统，采用前后端分离架构，支持多仓库、多商户的库存管理，提供完整的入库、出库、移库、盘点等功能。本项目基于若依框架进行开发和完善。

### 🎯 核心特性

- **多仓库管理**：支持多个仓库的独立管理
- **订单管理**：入库单、出库单、移库单、盘点单的全生命周期管理
- **库存管理**：实时库存查询、库存统计、库存历史记录
- **基础数据管理**：物料、仓库、商户、品牌、分类等基础数据维护
- **权限控制**：基于Sa-Token的细粒度权限管理
- **打印功能**：支持网页打印各种单据
- **数据导入导出**：支持Excel数据的导入导出

### 🏗️ 技术架构

**后端技术栈：**
- Java 17
- Spring Boot 3.2.6
- MyBatis-Plus 3.5.6
- MySQL 8.0
- Redis (缓存)
- Sa-Token 1.37.0 (权限认证)
- Swagger/OpenAPI 2.5.0 (接口文档)

**前端技术栈：**
- Vue 3
- Vite 3.2.3
- Element Plus
- Axios (HTTP客户端)
- Pinia (状态管理)

---

## 📁 项目结构

### 后端项目结构

```
wms-ruoyi/
├── ruoyi-admin-wms/            # 主应用入口
│   └── src/main/java/com/ruoyi/
│       ├── RuoYiApplication.java    # 启动类
│       └── wms/                     # WMS业务模块
│           ├── controller/          # 控制器层
│           ├── service/             # 服务层
│           ├── mapper/              # 数据访问层
│           ├── domain/              # 实体和VO/BO
│           └── config/              # 配置类
│
├── ruoyi-common/               # 通用模块
│   ├── ruoyi-common-core/       # 核心工具类
│   ├── ruoyi-common-mybatis/    # MyBatis封装
│   ├── ruoyi-common-redis/      # Redis工具
│   ├── ruoyi-common-satoken/    # Sa-Token认证
│   ├── ruoyi-common-excel/      # Excel工具
│   ├── ruoyi-common-oss/        # 对象存储
│   ├── ruoyi-common-sms/        # 短信服务
│   └── ...                      # 其他通用模块
│
├── ruoyi-modules/              # 业务模块
│   ├── ruoyi-system/           # 系统管理模块
│   ├── ruoyi-generator/        # 代码生成器
│   └── ruoyi-demo/             # 示例模块
│
├── script/                     # 脚本文件
│   ├── sql/                    # SQL脚本
│   └── bin/                    # 启动脚本
│
└── docs/                        # 文档目录
```

### 前端项目结构

```
ruoyi-wms-vue/
├── src/
│   ├── api/                    # API接口定义
│   │   ├── system/             # 系统相关接口
│   │   ├── wms/                # WMS业务接口
│   │   └── login.js            # 登录接口
│   │
│   ├── assets/                 # 静态资源
│   ├── components/             # 公共组件
│   ├── layout/                 # 布局组件
│   ├── router/                 # 路由配置
│   ├── store/                  # 状态管理
│   ├── utils/                  # 工具函数
│   ├── views/                  # 页面组件
│   │   ├── wms/                # WMS业务页面
│   │   │   ├── order/          # 订单管理
│   │   │   │   ├── receipt/    # 入库单
│   │   │   │   ├── shipment/   # 出库单
│   │   │   │   ├── movement/   # 移库单
│   │   │   │   └── check/      # 盘点单
│   │   │   ├── basic/          # 基础数据
│   │   │   ├── inventory/      # 库存管理
│   │   │   └── statistic/      # 统计报表
│   │   ├── system/             # 系统管理
│   │   └── dashboard/          # 仪表板
│   │
│   ├── App.vue                 # 根组件
│   └── main.js                 # 入口文件
│
├── public/                     # 公共资源
├── vite.config.js              # Vite配置
├── package.json                # 依赖配置
└── .env.development            # 开发环境配置
```

---

## 🗄️ 数据库设计

### 数据库信息

- **数据库名称**：`ry-vue`
- **字符集**：utf8mb4
- **用户名**：`root`
- **密码**：`Raychiu123`

### 核心数据表

#### 1. 系统管理表

| 表名 | 说明 | 主要字段 |
|------|------|----------|
| `sys_user` | 用户表 | user_id, user_name, nick_name, email, phonenumber |
| `sys_role` | 角色表 | role_id, role_name, role_key, data_scope |
| `sys_menu` | 菜单表 | menu_id, menu_name, parent_id, path, component |
| `sys_dept` | 部门表 | dept_id, dept_name, parent_id, leader |
| `sys_config` | 参数配置表 | config_id, config_name, config_key, config_value |

#### 2. WMS业务表

| 表名 | 说明 | 主要字段 |
|------|------|----------|
| `wms_warehouse` | 仓库表 | id, warehouse_name, warehouse_code, address, status |
| `wms_merchant` | 商户表 | id, merchant_name, merchant_code, contact_person, phone |
| `wms_item` | 物料表 | id, item_name, item_code, category_id, brand_id |
| `wms_item_category` | 物料分类表 | id, category_name, category_code, parent_id |
| `wms_item_brand` | 物料品牌表 | id, brand_name, brand_code |
| `wms_item_sku` | 物料SKU表 | id, item_id, sku_code, spec, unit |
| `wms_receipt_order` | 入库单表 | id, order_no, opt_type, merchant_id, order_status |
| `wms_receipt_order_detail` | 入库单明细表 | id, order_id, sku_id, quantity, price |
| `wms_shipment_order` | 出库单表 | id, order_no, opt_type, merchant_id, order_status |
| `wms_shipment_order_detail` | 出库单明细表 | id, order_id, sku_id, quantity, price |
| `wms_movement_order` | 移库单表 | id, order_no, from_warehouse, to_warehouse, order_status |
| `wms_movement_order_detail` | 移库单明细表 | id, order_id, sku_id, quantity |
| `wms_check_order` | 盘点单表 | id, order_no, warehouse_id, order_status |
| `wms_check_order_detail` | 盘点单明细表 | id, order_id, sku_id, system_qty, check_qty |
| `wms_inventory` | 库存表 | id, warehouse_id, sku_id, quantity, lock_quantity |
| `wms_inventory_history` | 库存历史表 | id, warehouse_id, sku_id, quantity, operation_type, order_id |

### 订单状态说明

**入库单状态 (wms_receipt_order.order_status)：**
- 0: 暂存
- 1: 已审核
- 2: 部分入库
- 3: 已完成
- 4: 已作废

**出库单状态 (wms_shipment_order.order_status)：**
- 0: 暂存
- 1: 已审核
- 2: 部分出库
- 3: 已完成
- 4: 已作废

**操作类型 (opt_type)：**
- 1: 采购入库
- 2: 外协入库
- 3: 退货入库
- 4: 销售出库
- 5: 外协出库
- 6: 调拨出库

---

## 🔌 API接口文档

### 基础信息

- **基础URL**：http://localhost:8080
- **接口前缀**：`/wms`
- **认证方式**：Sa-Token (Header: `Authorization: Bearer {token}`)
- **数据格式**：JSON
- **接口文档**：http://localhost:8080/doc.html

### 核心接口列表

#### 1. 入库管理接口

| 接口路径 | 方法 | 说明 | 权限标识 |
|----------|------|------|----------|
| `/wms/receiptOrder/list` | GET | 查询入库单列表 | wms:receipt:all |
| `/wms/receiptOrder/{id}` | GET | 获取入库单详情 | wms:receipt:query |
| `/wms/receiptOrder` | POST | 新增入库单 | wms:receipt:add |
| `/wms/receiptOrder` | PUT | 修改入库单 | wms:receipt:edit |
| `/wms/receiptOrder/{ids}` | DELETE | 删除入库单 | wms:receipt:remove |
| `/wms/receiptOrder/export` | POST | 导出入库单 | wms:receipt:export |
| `/wms/receiptOrder/detail/list` | GET | 查询入库单明细 | wms:receipt:all |

#### 2. 出库管理接口

| 接口路径 | 方法 | 说明 | 权限标识 |
|----------|------|------|----------|
| `/wms/shipmentOrder/list` | GET | 查询出库单列表 | wms:shipment:all |
| `/wms/shipmentOrder/{id}` | GET | 获取出库单详情 | wms:shipment:query |
| `/wms/shipmentOrder` | POST | 新增出库单 | wms:shipment:add |
| `/wms/shipmentOrder` | PUT | 修改出库单 | wms:shipment:edit |
| `/wms/shipmentOrder/{ids}` | DELETE | 删除出库单 | wms:shipment:remove |
| `/wms/shipmentOrder/export` | POST | 导出出库单 | wms:shipment:export |
| `/wms/shipmentOrder/detail/list` | GET | 查询出库单明细 | wms:shipment:all |

#### 3. 库存管理接口

| 接口路径 | 方法 | 说明 | 权限标识 |
|----------|------|------|----------|
| `/wms/inventory/list` | GET | 查询库存列表 | wms:inventory:all |
| `/wms/inventory/statistic` | GET | 库存统计 | wms:inventory:all |
| `/wms/inventoryHistory/list` | GET | 查询库存历史 | wms:inventory:all |
| `/wms/inventoryHistory/export` | POST | 导出库存历史 | wms:inventory:export |

#### 4. 基础数据接口

| 接口路径 | 方法 | 说明 | 权限标识 |
|----------|------|------|----------|
| `/wms/warehouse/list` | GET | 查询仓库列表 | wms:warehouse:all |
| `/wms/merchant/list` | GET | 查询商户列表 | wms:merchant:all |
| `/wms/item/list` | GET | 查询物料列表 | wms:item:all |
| `/wms/itemCategory/list` | GET | 查询物料分类 | wms:category:all |
| `/wms/itemBrand/list` | GET | 查询物料品牌 | wms:brand:all |
| `/wms/itemSku/list` | GET | 查询物料SKU | wms:sku:all |

#### 5. 移库和盘点接口

| 接口路径 | 方法 | 说明 | 权限标识 |
|----------|------|------|----------|
| `/wms/movementOrder/list` | GET | 查询移库单列表 | wms:movement:all |
| `/wms/movementOrder` | POST | 新增移库单 | wms:movement:add |
| `/wms/checkOrder/list` | GET | 查询盘点单列表 | wms:check:all |
| `/wms/checkOrder` | POST | 新增盘点单 | wms:check:add |
| `/wms/checkOrder/{id}/complete` | POST | 完成盘点 | wms:check:edit |

### 接口响应格式

**成功响应：**
```json
{
  "code": 200,
  "msg": "操作成功",
  "data": {
    // 返回数据
  }
}
```

**失败响应：**
```json
{
  "code": 500,
  "msg": "错误信息",
  "data": null
}
```

**分页响应：**
```json
{
  "code": 200,
  "msg": "查询成功",
  "rows": [
    // 数据列表
  ],
  "total": 100
}
```

---

## ⚙️ 技术配置

### 后端配置

#### 1. 环境要求

- **JDK**: 17+
- **Maven**: 3.6+
- **MySQL**: 8.0+
- **Redis**: 6.0+

#### 2. 配置文件说明

**主配置文件** (`application.yml`)：
```yaml
ruoyi:
  name: ruoyi-wms-service
  version: 5.2.0

server:
  port: 8080
  servlet:
    context-path: /

spring:
  profiles:
    active: dev

sa-token:
  token-name: Authorization
  timeout: 86400
  token-prefix: "Bearer"
```

**开发环境配置** (`application-dev.yml`)：
```yaml
spring:
  datasource:
    dynamic:
      primary: master
      datasource:
        master:
          url: jdbc:mysql://localhost:3306/ry-vue?useUnicode=true&characterEncoding=utf8&zeroDateTimeBehavior=convertToNull&useSSL=false&serverTimezone=GMT%2B8&autoReconnect=true&rewriteBatchedStatements=true
          username: root
          password: Raychiu123
          driver-class-name: com.mysql.cj.jdbc.Driver

  data:
    redis:
      host: localhost
      port: 6379
      database: 0
```

#### 3. 构建和运行

**构建项目：**
```bash
cd wms-ruoyi
export JAVA_HOME=/opt/homebrew/opt/openjdk@17
export PATH="$JAVA_HOME/bin:$PATH"
mvn clean install -DskipTests
```

**运行项目：**
```bash
export JAVA_HOME=/opt/homebrew/opt/openjdk@17
export PATH="$JAVA_HOME/bin:$PATH"
mvn spring-boot:run -pl ruoyi-admin-wms
```

### 前端配置

#### 1. 环境要求

- **Node.js**: 16+
- **npm**: 8+

#### 2. 配置文件说明

**开发环境配置** (`.env.development`)：
```env
VITE_APP_TITLE = ruoyi-wms后台管理系统
VITE_APP_ENV = 'development'
VITE_APP_BASE_API = '/dev-api'
VITE_APP_CONTEXT_PATH = '/'
```

**Vite配置** (`vite.config.js`)：
```javascript
export default defineConfig({
  base: env.VITE_APP_CONTEXT_PATH,
  server: {
    port: 80,
    host: true,
    open: true,
    proxy: {
      '/dev-api': {
        target: 'http://localhost:8080',
        changeOrigin: true,
        rewrite: (p) => p.replace(/^\/dev-api/, '')
      }
    }
  }
})
```

#### 3. 构建和运行

**安装依赖：**
```bash
cd ruoyi-wms-vue
npm install
```

**开发运行：**
```bash
npm run dev
```

**生产构建：**
```bash
npm run build
```

---

## 🚀 快速开始

### 1. 数据库初始化

```bash
# 创建数据库
mysql -u root -p
CREATE DATABASE ry_vue CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;

# 导入SQL脚本
mysql -u root -p ry_vue < wms-ruoyi/script/sql/wms.sql
```

### 2. 后端启动

```bash
cd wms-ruoyi
export JAVA_HOME=/opt/homebrew/opt/openjdk@17
export PATH="$JAVA_HOME/bin:$PATH"
mvn clean install -DskipTests
mvn spring-boot:run -pl ruoyi-admin-wms
```

### 3. 前端启动

```bash
cd ruoyi-wms-vue
npm install
npm run dev
```

### 4. 访问系统

- **前端地址**: http://localhost:81/
- **后端接口**: http://localhost:8080/
- **接口文档**: http://localhost:8080/doc.html
- **默认用户**: admin / admin123

---

## 🐛 常见问题

### 1. 后端启动失败

**问题**：连接数据库失败
**解决**：
- 检查MySQL服务是否启动
- 检查数据库配置是否正确
- 确认数据库用户名和密码
- 检查数据库字符集是否为utf8mb4

### 2. 前端启动失败

**问题**：npm install 失败
**解决**：
- 检查Node.js版本是否>=16
- 清除缓存：`npm cache clean --force`
- 删除node_modules重新安装

### 3. 接口跨域问题

**问题**：前端访问后端接口出现跨域错误
**解决**：
- 检查vite.config.js中的proxy配置
- 确认后端CORS配置是否正确

### 4. 登录失败

**问题**：登录时提示"认证失败"
**解决**：
- 检查用户名和密码是否正确
- 确认Redis服务是否启动
- 检查Sa-Token配置是否正确

---

## 📞 技术支持

- **问题反馈**：提交Issue到GitHub
- **在线体验**：https://wms.ichengle.top/

---

## 📄 开源协议

本项目基于 [MIT License](LICENSE) 开源协议，欢迎个人和企业使用。

---

*最后更新时间：2026-04-08*
