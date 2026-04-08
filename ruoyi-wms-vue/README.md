# WMS 仓库管理系统

## 项目简介

WMS (Warehouse Management System) 是一个功能完善的仓库管理系统，采用前后端分离架构开发。系统支持多仓库、多商户的库存管理，提供入库、出库、移库、盘点等核心功能，帮助企业实现仓库数字化管理。

## 技术架构

### 后端技术栈
- **Java 17**
- **Spring Boot 3.2.6**
- **MyBatis-Plus 3.5.6**
- **MySQL 8.0**
- **Redis**
- **Sa-Token 1.37.0**

### 前端技术栈
- **Vue 3**
- **Vite 3.2.3**
- **Element Plus**
- **Pinia**
- **Axios**

## 核心功能

- **仓库管理**：支持多仓库、多库区、多货位的层级管理
- **入库管理**：采购入库、退货入库、其他入库等多种入库类型
- **出库管理**：销售出库、领料出库、其他出库等多种出库类型
- **移库管理**：支持库存在不同仓库或货位间的转移
- **盘点管理**：定期盘点、实时盘点，支持盘点差异处理
- **库存管理**：实时库存查询、库存预警、库存流水追溯
- **基础数据**：商品、品牌、分类、商户等基础信息管理
- **权限管理**：基于角色的权限控制系统

## 快速开始

### 环境要求
- JDK 17+
- Node.js 16+
- MySQL 8.0+
- Redis

### 后端启动
```bash
cd wms-ruoyi
mvn spring-boot:run -pl ruoyi-admin-wms
```

### 前端启动
```bash
cd ruoyi-wms-vue
npm install
npm run dev
```

### 访问地址
- 前端系统：http://localhost:81/
- 后端接口：http://localhost:8080/
- API文档：http://localhost:8080/doc.html
- 默认账号：admin / admin123

## 数据库配置

```sql
-- 创建数据库
CREATE DATABASE `ry-vue` DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

-- 导入初始化脚本
mysql -u root -p ry-vue < wms-ruoyi/script/sql/wms.sql
```

## 项目结构

```
wms-ruoyi/
├── ruoyi-admin-wms/         # 主应用入口
├── ruoyi-common/            # 通用模块
├── ruoyi-modules/           # 业务模块
└── script/                  # SQL脚本

ruoyi-wms-vue/
├── src/
│   ├── api/                 # API接口
│   ├── views/               # 页面组件
│   ├── components/          # 公共组件
│   └── utils/               # 工具函数
└── public/                  # 静态资源
```

## 系统特点

- **前后端分离**：独立的开发和部署，提高开发效率
- **响应式设计**：支持PC端、平板、移动端访问
- **权限控制**：细粒度的权限管理，保障数据安全
- **高性能**：采用Redis缓存，提升系统响应速度
- **易扩展**：模块化设计，方便功能扩展

## 许可证

MIT License
