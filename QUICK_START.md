# WMS 仓库管理系统 - 快速开始指南

## 🚀 5分钟快速启动

### 前置条件
- Java 17+
- Node.js 16+
- MySQL 8.0+
- Redis 6.0+

### 1. 数据库配置
```bash
# 创建数据库
mysql -u root -p
CREATE DATABASE ry_vue CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;

# 导入数据
mysql -u root -p ry_vue < wms-ruoyi/script/sql/wms.sql
```

### 2. 启动后端
```bash
cd wms-ruoyi

# 设置Java环境
export JAVA_HOME=/opt/homebrew/opt/openjdk@17
export PATH="$JAVA_HOME/bin:$PATH"

# 安装依赖（首次）
mvn clean install -DskipTests

# 启动服务
mvn spring-boot:run -pl ruoyi-admin-wms
```

### 3. 启动前端
```bash
cd ruoyi-wms-vue

# 安装依赖
npm install

# 启动开发服务器
npm run dev
```

### 4. 访问系统
- **前端地址**: http://localhost:81/
- **默认账号**: admin / admin123
- **API文档**: http://localhost:8080/doc.html

---

## 📋 核心功能

### 1. 入库管理
- 采购入库、外协入库、退货入库
- 入库单审核、入库明细管理
- 支持打印入库单

### 2. 出库管理
- 销售出库、外协出库、调拨出库
- 出库单审核、库存扣减
- 支持打印出库单

### 3. 库存管理
- 实时库存查询
- 库存历史记录
- 库存统计报表

### 4. 移库管理
- 仓库间库存转移
- 移库单审核
- 移库历史查询

### 5. 盘点管理
- 盘点单创建
- 盘点数据录入
- 盘点差异处理

---

## 🔧 配置说明

### 后端配置文件
- `wms-ruoyi/ruoyi-admin-wms/src/main/resources/application.yml` - 主配置
- `wms-ruoyi/ruoyi-admin-wms/src/main/resources/application-dev.yml` - 开发环境配置
- 修改数据库连接信息、Redis配置

### 前端配置文件
- `ruoyi-wms-vue/.env.development` - 开发环境配置
- `ruoyi-wms-vue/vite.config.js` - Vite构建配置
- 修改API接口地址、代理设置

---

## 🎯 常用命令

### 后端
```bash
# 编译
cd wms-ruoyi
mvn clean compile

# 安装到本地仓库
mvn clean install -DskipTests

# 运行
mvn spring-boot:run -pl ruoyi-admin-wms

# 打包
mvn clean package -DskipTests
```

### 前端
```bash
# 安装依赖
cd ruoyi-wms-vue
npm install

# 开发运行
npm run dev

# 生产构建
npm run build

# 预览构建结果
npm run preview
```

---

## 📞 遇到问题？

1. **后端启动失败**：检查Java版本、数据库连接、Redis服务
2. **前端启动失败**：检查Node版本、依赖安装、代理配置
3. **登录失败**：检查Redis服务、用户密码、数据库连接
4. **端口占用**：修改配置文件中的端口号或停止占用端口的进程

---

## 📖 详细文档

- **项目文档**: 查看 `PROJECT_DOCUMENTATION.md`
- **API文档**: 查看 `API_DOCUMENTATION.md`
- **数据库设计**: 查看 `DATABASE_DESIGN.md`

---

*最后更新: 2026-04-08*
