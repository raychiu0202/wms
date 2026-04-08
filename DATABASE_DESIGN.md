# WMS系统数据库设计文档

## 📋 数据库信息

- **数据库名称**: `ry-vue`
- **字符集**: `utf8mb4`
- **排序规则**: `utf8mb4_general_ci`
- **连接用户**: `root`
- **连接密码**: `Raychiu123`
- **初始化脚本**: `wms-ruoyi/script/sql/wms.sql`

---

## 🗄️ 数据表分类

### 1. 系统管理表 (sys_*)
### 2. WMS业务表 (wms_*)
### 3. 代码生成表 (gen_*)

---

## 🔧 系统管理表

### sys_user - 用户表
```sql
CREATE TABLE `sys_user` (
  `user_id` bigint NOT NULL AUTO_INCREMENT COMMENT '用户ID',
  `tenant_id` varchar(20) DEFAULT '000000' COMMENT '租户编号',
  `user_name` varchar(30) NOT NULL COMMENT '用户账号',
  `nick_name` varchar(30) NOT NULL COMMENT '用户昵称',
  `user_type` varchar(2) DEFAULT '00' COMMENT '用户类型（00系统用户）',
  `email` varchar(50) DEFAULT '' COMMENT '用户邮箱',
  `phonenumber` varchar(11) DEFAULT '' COMMENT '手机号码',
  `sex` char(1) DEFAULT '0' COMMENT '用户性别（0男 1女 2未知）',
  `avatar` varchar(100) DEFAULT '' COMMENT '头像地址',
  `password` varchar(100) DEFAULT '' COMMENT '密码',
  `status` char(1) DEFAULT '0' COMMENT '帐号状态（0正常 1停用）',
  `del_flag` char(1) DEFAULT '0' COMMENT '删除标志（0代表存在 2代表删除）',
  `login_ip` varchar(128) DEFAULT '' COMMENT '最后登录IP',
  `login_date` datetime DEFAULT NULL COMMENT '最后登录时间',
  `create_by` bigint DEFAULT NULL COMMENT '创建者',
  `create_time` datetime DEFAULT NULL COMMENT '创建时间',
  `update_by` bigint DEFAULT NULL COMMENT '更新者',
  `update_time` datetime DEFAULT NULL COMMENT '更新时间',
  `remark` varchar(500) DEFAULT NULL COMMENT '备注',
  PRIMARY KEY (`user_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='用户信息表';
```

### sys_role - 角色表
```sql
CREATE TABLE `sys_role` (
  `role_id` bigint NOT NULL AUTO_INCREMENT COMMENT '角色ID',
  `tenant_id` varchar(20) DEFAULT '000000' COMMENT '租户编号',
  `role_name` varchar(30) NOT NULL COMMENT '角色名称',
  `role_key` varchar(100) NOT NULL COMMENT '角色权限字符串',
  `role_sort` int NOT NULL COMMENT '显示顺序',
  `data_scope` char(1) DEFAULT '1' COMMENT '数据范围（1：全部数据权限 2：自定义数据权限 3：本部门数据权限 4：本部门及以下数据权限）',
  `menu_check_strictly` tinyint(1) DEFAULT 1 COMMENT '菜单树选择项是否关联显示',
  `dept_check_strictly` tinyint(1) DEFAULT 1 COMMENT '部门树选择项是否关联显示',
  `status` char(1) NOT NULL DEFAULT '0' COMMENT '角色状态（0正常 1停用）',
  `del_flag` char(1) DEFAULT '0' COMMENT '删除标志（0代表存在 2代表删除）',
  `create_by` bigint DEFAULT NULL COMMENT '创建者',
  `create_time` datetime DEFAULT NULL COMMENT '创建时间',
  `update_by` bigint DEFAULT NULL COMMENT '更新者',
  `update_time` datetime DEFAULT NULL COMMENT '更新时间',
  `remark` varchar(500) DEFAULT NULL COMMENT '备注',
  PRIMARY KEY (`role_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='角色信息表';
```

### sys_menu - 菜单表
```sql
CREATE TABLE `sys_menu` (
  `menu_id` bigint NOT NULL AUTO_INCREMENT COMMENT '菜单ID',
  `menu_name` varchar(50) NOT NULL COMMENT '菜单名称',
  `parent_id` bigint DEFAULT 0 COMMENT '父菜单ID',
  `order_num` int DEFAULT 0 COMMENT '显示顺序',
  `path` varchar(200) DEFAULT '' COMMENT '路由地址',
  `component` varchar(255) DEFAULT NULL COMMENT '组件路径',
  `query` varchar(255) DEFAULT NULL COMMENT '路由参数',
  `route_name` varchar(50) DEFAULT '' COMMENT '路由名称',
  `is_frame` int DEFAULT 1 COMMENT '是否为外链（0是 1否）',
  `is_cache` int DEFAULT 0 COMMENT '是否缓存（0缓存 1不缓存）',
  `menu_type` char(1) DEFAULT '' COMMENT '菜单类型（M目录 C菜单 F按钮）',
  `visible` char(1) DEFAULT '0' COMMENT '显示状态（0显示 1隐藏）',
  `status` char(1) DEFAULT '0' COMMENT '菜单状态（0正常 1停用）',
  `perms` varchar(100) DEFAULT NULL COMMENT '权限标识',
  `icon` varchar(100) DEFAULT '#' COMMENT '菜单图标',
  `create_by` bigint DEFAULT NULL COMMENT '创建者',
  `create_time` datetime DEFAULT NULL COMMENT '创建时间',
  `update_by` bigint DEFAULT NULL COMMENT '更新者',
  `update_time` datetime DEFAULT NULL COMMENT '更新时间',
  `remark` varchar(500) DEFAULT '' COMMENT '备注',
  PRIMARY KEY (`menu_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='菜单权限表';
```

### sys_dept - 部门表
```sql
CREATE TABLE `sys_dept` (
  `dept_id` bigint NOT NULL AUTO_INCREMENT COMMENT '部门id',
  `tenant_id` varchar(20) DEFAULT '000000' COMMENT '租户编号',
  `parent_id` bigint DEFAULT 0 COMMENT '父部门id',
  `ancestors` varchar(500) DEFAULT '' COMMENT '祖级列表',
  `dept_name` varchar(30) DEFAULT '' COMMENT '部门名称',
  `order_num` int DEFAULT 0 COMMENT '显示顺序',
  `leader` varchar(20) DEFAULT NULL COMMENT '负责人',
  `phone` varchar(11) DEFAULT NULL COMMENT '联系电话',
  `email` varchar(50) DEFAULT NULL COMMENT '邮箱',
  `status` char(1) DEFAULT '0' COMMENT '部门状态（0正常 1停用）',
  `del_flag` char(1) DEFAULT '0' COMMENT '删除标志（0代表存在 2代表删除）',
  `create_by` bigint DEFAULT NULL COMMENT '创建者',
  `create_time` datetime DEFAULT NULL COMMENT '创建时间',
  `update_by` bigint DEFAULT NULL COMMENT '更新者',
  `update_time` datetime DEFAULT NULL COMMENT '更新时间',
  PRIMARY KEY (`dept_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='部门表';
```

---

## 📦 WMS业务表

### wms_warehouse - 仓库表
```sql
CREATE TABLE `wms_warehouse` (
  `id` bigint NOT NULL AUTO_INCREMENT COMMENT '主键ID',
  `warehouse_name` varchar(100) NOT NULL COMMENT '仓库名称',
  `warehouse_code` varchar(50) NOT NULL COMMENT '仓库编码',
  `address` varchar(200) DEFAULT NULL COMMENT '仓库地址',
  `contact_person` varchar(50) DEFAULT NULL COMMENT '联系人',
  `contact_phone` varchar(20) DEFAULT NULL COMMENT '联系电话',
  `status` char(1) DEFAULT '0' COMMENT '状态（0正常 1停用）',
  `create_by` bigint DEFAULT NULL COMMENT '创建者',
  `create_time` datetime DEFAULT NULL COMMENT '创建时间',
  `update_by` bigint DEFAULT NULL COMMENT '更新者',
  `update_time` datetime DEFAULT NULL COMMENT '更新时间',
  `remark` varchar(500) DEFAULT NULL COMMENT '备注',
  PRIMARY KEY (`id`),
  UNIQUE KEY `uk_warehouse_code` (`warehouse_code`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='仓库表';
```

### wms_merchant - 商户表
```sql
CREATE TABLE `wms_merchant` (
  `id` bigint NOT NULL AUTO_INCREMENT COMMENT '主键ID',
  `merchant_name` varchar(100) NOT NULL COMMENT '商户名称',
  `merchant_code` varchar(50) NOT NULL COMMENT '商户编码',
  `contact_person` varchar(50) DEFAULT NULL COMMENT '联系人',
  `contact_phone` varchar(20) DEFAULT NULL COMMENT '联系电话',
  `email` varchar(100) DEFAULT NULL COMMENT '邮箱',
  `address` varchar(200) DEFAULT NULL COMMENT '地址',
  `merchant_type` char(1) DEFAULT '1' COMMENT '商户类型（1供应商 2客户）',
  `status` char(1) DEFAULT '0' COMMENT '状态（0正常 1停用）',
  `create_by` bigint DEFAULT NULL COMMENT '创建者',
  `create_time` datetime DEFAULT NULL COMMENT '创建时间',
  `update_by` bigint DEFAULT NULL COMMENT '更新者',
  `update_time` datetime DEFAULT NULL COMMENT '更新时间',
  `remark` varchar(500) DEFAULT NULL COMMENT '备注',
  PRIMARY KEY (`id`),
  UNIQUE KEY `uk_merchant_code` (`merchant_code`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='商户表';
```

### wms_item_category - 物料分类表
```sql
CREATE TABLE `wms_item_category` (
  `id` bigint NOT NULL AUTO_INCREMENT COMMENT '主键ID',
  `category_name` varchar(100) NOT NULL COMMENT '分类名称',
  `category_code` varchar(50) NOT NULL COMMENT '分类编码',
  `parent_id` bigint DEFAULT 0 COMMENT '父分类ID',
  `ancestors` varchar(500) DEFAULT '' COMMENT '祖级列表',
  `order_num` int DEFAULT 0 COMMENT '显示顺序',
  `status` char(1) DEFAULT '0' COMMENT '状态（0正常 1停用）',
  `create_by` bigint DEFAULT NULL COMMENT '创建者',
  `create_time` datetime DEFAULT NULL COMMENT '创建时间',
  `update_by` bigint DEFAULT NULL COMMENT '更新者',
  `update_time` datetime DEFAULT NULL COMMENT '更新时间',
  `remark` varchar(500) DEFAULT NULL COMMENT '备注',
  PRIMARY KEY (`id`),
  UNIQUE KEY `uk_category_code` (`category_code`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='物料分类表';
```

### wms_item_brand - 物料品牌表
```sql
CREATE TABLE `wms_item_brand` (
  `id` bigint NOT NULL AUTO_INCREMENT COMMENT '主键ID',
  `brand_name` varchar(100) NOT NULL COMMENT '品牌名称',
  `brand_code` varchar(50) NOT NULL COMMENT '品牌编码',
  `logo` varchar(200) DEFAULT NULL COMMENT '品牌logo',
  `status` char(1) DEFAULT '0' COMMENT '状态（0正常 1停用）',
  `create_by` bigint DEFAULT NULL COMMENT '创建者',
  `create_time` datetime DEFAULT NULL COMMENT '创建时间',
  `update_by` bigint DEFAULT NULL COMMENT '更新者',
  `update_time` datetime DEFAULT NULL COMMENT '更新时间',
  `remark` varchar(500) DEFAULT NULL COMMENT '备注',
  PRIMARY KEY (`id`),
  UNIQUE KEY `uk_brand_code` (`brand_code`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='物料品牌表';
```

### wms_item - 物料表
```sql
CREATE TABLE `wms_item` (
  `id` bigint NOT NULL AUTO_INCREMENT COMMENT '主键ID',
  `item_name` varchar(200) NOT NULL COMMENT '物料名称',
  `item_code` varchar(50) NOT NULL COMMENT '物料编码',
  `category_id` bigint DEFAULT NULL COMMENT '分类ID',
  `brand_id` bigint DEFAULT NULL COMMENT '品牌ID',
  `unit` varchar(20) DEFAULT NULL COMMENT '计量单位',
  `shelf_life` int DEFAULT NULL COMMENT '保质期（天）',
  `spec` varchar(200) DEFAULT NULL COMMENT '规格描述',
  `image` varchar(500) DEFAULT NULL COMMENT '图片地址',
  `status` char(1) DEFAULT '0' COMMENT '状态（0正常 1停用）',
  `create_by` bigint DEFAULT NULL COMMENT '创建者',
  `create_time` datetime DEFAULT NULL COMMENT '创建时间',
  `update_by` bigint DEFAULT NULL COMMENT '更新者',
  `update_time` datetime DEFAULT NULL COMMENT '更新时间',
  `remark` varchar(500) DEFAULT NULL COMMENT '备注',
  PRIMARY KEY (`id`),
  UNIQUE KEY `uk_item_code` (`item_code`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='物料表';
```

### wms_item_sku - 物料SKU表
```sql
CREATE TABLE `wms_item_sku` (
  `id` bigint NOT NULL AUTO_INCREMENT COMMENT '主键ID',
  `item_id` bigint NOT NULL COMMENT '物料ID',
  `sku_code` varchar(50) NOT NULL COMMENT 'SKU编码',
  `sku_name` varchar(200) DEFAULT NULL COMMENT 'SKU名称',
  `spec` varchar(200) DEFAULT NULL COMMENT '规格',
  `unit` varchar(20) DEFAULT NULL COMMENT '单位',
  `barcode` varchar(50) DEFAULT NULL COMMENT '条形码',
  `price` decimal(10,2) DEFAULT NULL COMMENT '参考价格',
  `weight` decimal(10,2) DEFAULT NULL COMMENT '重量',
  `volume` decimal(10,2) DEFAULT NULL COMMENT '体积',
  `status` char(1) DEFAULT '0' COMMENT '状态（0正常 1停用）',
  `create_by` bigint DEFAULT NULL COMMENT '创建者',
  `create_time` datetime DEFAULT NULL COMMENT '创建时间',
  `update_by` bigint DEFAULT NULL COMMENT '更新者',
  `update_time` datetime DEFAULT NULL COMMENT '更新时间',
  `remark` varchar(500) DEFAULT NULL COMMENT '备注',
  PRIMARY KEY (`id`),
  UNIQUE KEY `uk_sku_code` (`sku_code`),
  KEY `idx_item_id` (`item_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='物料SKU表';
```

### wms_receipt_order - 入库单表
```sql
CREATE TABLE `wms_receipt_order` (
  `id` bigint NOT NULL AUTO_INCREMENT COMMENT '主键ID',
  `order_no` varchar(50) NOT NULL COMMENT '入库单号',
  `opt_type` int NOT NULL COMMENT '操作类型（1采购入库 2外协入库 3退货入库）',
  `merchant_id` bigint DEFAULT NULL COMMENT '商户ID',
  `warehouse_id` bigint NOT NULL COMMENT '仓库ID',
  `order_status` int DEFAULT 0 COMMENT '单据状态（0暂存 1已审核 2部分入库 3已完成 4已作废）',
  `total_qty` int DEFAULT 0 COMMENT '总数量',
  `total_amount` decimal(15,2) DEFAULT 0.00 COMMENT '总金额',
  `receipt_date` date DEFAULT NULL COMMENT '入库日期',
  `operator_id` bigint DEFAULT NULL COMMENT '操作员ID',
  `operator_name` varchar(50) DEFAULT NULL COMMENT '操作员姓名',
  `audit_by` bigint DEFAULT NULL COMMENT '审核人ID',
  `audit_time` datetime DEFAULT NULL COMMENT '审核时间',
  `create_by` bigint DEFAULT NULL COMMENT '创建者',
  `create_time` datetime DEFAULT NULL COMMENT '创建时间',
  `update_by` bigint DEFAULT NULL COMMENT '更新者',
  `update_time` datetime DEFAULT NULL COMMENT '更新时间',
  `remark` varchar(500) DEFAULT NULL COMMENT '备注',
  PRIMARY KEY (`id`),
  UNIQUE KEY `uk_order_no` (`order_no`),
  KEY `idx_merchant_id` (`merchant_id`),
  KEY `idx_warehouse_id` (`warehouse_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='入库单表';
```

### wms_receipt_order_detail - 入库单明细表
```sql
CREATE TABLE `wms_receipt_order_detail` (
  `id` bigint NOT NULL AUTO_INCREMENT COMMENT '主键ID',
  `order_id` bigint NOT NULL COMMENT '入库单ID',
  `sku_id` bigint NOT NULL COMMENT 'SKU ID',
  `sku_code` varchar(50) DEFAULT NULL COMMENT 'SKU编码',
  `sku_name` varchar(200) DEFAULT NULL COMMENT 'SKU名称',
  `quantity` int NOT NULL COMMENT '数量',
  `price` decimal(10,2) DEFAULT NULL COMMENT '单价',
  `amount` decimal(15,2) DEFAULT NULL COMMENT '金额',
  `batch_no` varchar(50) DEFAULT NULL COMMENT '批次号',
  `production_date` date DEFAULT NULL COMMENT '生产日期',
  `expiry_date` date DEFAULT NULL COMMENT '有效期',
  `create_by` bigint DEFAULT NULL COMMENT '创建者',
  `create_time` datetime DEFAULT NULL COMMENT '创建时间',
  `update_by` bigint DEFAULT NULL COMMENT '更新者',
  `update_time` datetime DEFAULT NULL COMMENT '更新时间',
  `remark` varchar(500) DEFAULT NULL COMMENT '备注',
  PRIMARY KEY (`id`),
  KEY `idx_order_id` (`order_id`),
  KEY `idx_sku_id` (`sku_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='入库单明细表';
```

### wms_shipment_order - 出库单表
```sql
CREATE TABLE `wms_shipment_order` (
  `id` bigint NOT NULL AUTO_INCREMENT COMMENT '主键ID',
  `order_no` varchar(50) NOT NULL COMMENT '出库单号',
  `opt_type` int NOT NULL COMMENT '操作类型（4销售出库 5外协出库 6调拨出库）',
  `merchant_id` bigint DEFAULT NULL COMMENT '商户ID',
  `warehouse_id` bigint NOT NULL COMMENT '仓库ID',
  `order_status` int DEFAULT 0 COMMENT '单据状态（0暂存 1已审核 2部分出库 3已完成 4已作废）',
  `total_qty` int DEFAULT 0 COMMENT '总数量',
  `total_amount` decimal(15,2) DEFAULT 0.00 COMMENT '总金额',
  `shipment_date` date DEFAULT NULL COMMENT '出库日期',
  `operator_id` bigint DEFAULT NULL COMMENT '操作员ID',
  `operator_name` varchar(50) DEFAULT NULL COMMENT '操作员姓名',
  `audit_by` bigint DEFAULT NULL COMMENT '审核人ID',
  `audit_time` datetime DEFAULT NULL COMMENT '审核时间',
  `create_by` bigint DEFAULT NULL COMMENT '创建者',
  `create_time` datetime DEFAULT NULL COMMENT '创建时间',
  `update_by` bigint DEFAULT NULL COMMENT '更新者',
  `update_time` datetime DEFAULT NULL COMMENT '更新时间',
  `remark` varchar(500) DEFAULT NULL COMMENT '备注',
  PRIMARY KEY (`id`),
  UNIQUE KEY `uk_order_no` (`order_no`),
  KEY `idx_merchant_id` (`merchant_id`),
  KEY `idx_warehouse_id` (`warehouse_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='出库单表';
```

### wms_shipment_order_detail - 出库单明细表
```sql
CREATE TABLE `wms_shipment_order_detail` (
  `id` bigint NOT NULL AUTO_INCREMENT COMMENT '主键ID',
  `order_id` bigint NOT NULL COMMENT '出库单ID',
  `sku_id` bigint NOT NULL COMMENT 'SKU ID',
  `sku_code` varchar(50) DEFAULT NULL COMMENT 'SKU编码',
  `sku_name` varchar(200) DEFAULT NULL COMMENT 'SKU名称',
  `quantity` int NOT NULL COMMENT '数量',
  `price` decimal(10,2) DEFAULT NULL COMMENT '单价',
  `amount` decimal(15,2) DEFAULT NULL COMMENT '金额',
  `batch_no` varchar(50) DEFAULT NULL COMMENT '批次号',
  `create_by` bigint DEFAULT NULL COMMENT '创建者',
  `create_time` datetime DEFAULT NULL COMMENT '创建时间',
  `update_by` bigint DEFAULT NULL COMMENT '更新者',
  `update_time` datetime DEFAULT NULL COMMENT '更新时间',
  `remark` varchar(500) DEFAULT NULL COMMENT '备注',
  PRIMARY KEY (`id`),
  KEY `idx_order_id` (`order_id`),
  KEY `idx_sku_id` (`sku_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='出库单明细表';
```

### wms_movement_order - 移库单表
```sql
CREATE TABLE `wms_movement_order` (
  `id` bigint NOT NULL AUTO_INCREMENT COMMENT '主键ID',
  `order_no` varchar(50) NOT NULL COMMENT '移库单号',
  `from_warehouse` bigint NOT NULL COMMENT '源仓库ID',
  `to_warehouse` bigint NOT NULL COMMENT '目标仓库ID',
  `order_status` int DEFAULT 0 COMMENT '单据状态（0暂存 1已审核 2部分移库 3已完成 4已作废）',
  `total_qty` int DEFAULT 0 COMMENT '总数量',
  `movement_date` date DEFAULT NULL COMMENT '移库日期',
  `operator_id` bigint DEFAULT NULL COMMENT '操作员ID',
  `operator_name` varchar(50) DEFAULT NULL COMMENT '操作员姓名',
  `audit_by` bigint DEFAULT NULL COMMENT '审核人ID',
  `audit_time` datetime DEFAULT NULL COMMENT '审核时间',
  `create_by` bigint DEFAULT NULL COMMENT '创建者',
  `create_time` datetime DEFAULT NULL COMMENT '创建时间',
  `update_by` bigint DEFAULT NULL COMMENT '更新者',
  `update_time` datetime DEFAULT NULL COMMENT '更新时间',
  `remark` varchar(500) DEFAULT NULL COMMENT '备注',
  PRIMARY KEY (`id`),
  UNIQUE KEY `uk_order_no` (`order_no`),
  KEY `idx_from_warehouse` (`from_warehouse`),
  KEY `idx_to_warehouse` (`to_warehouse`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='移库单表';
```

### wms_movement_order_detail - 移库单明细表
```sql
CREATE TABLE `wms_movement_order_detail` (
  `id` bigint NOT NULL AUTO_INCREMENT COMMENT '主键ID',
  `order_id` bigint NOT NULL COMMENT '移库单ID',
  `sku_id` bigint NOT NULL COMMENT 'SKU ID',
  `sku_code` varchar(50) DEFAULT NULL COMMENT 'SKU编码',
  `sku_name` varchar(200) DEFAULT NULL COMMENT 'SKU名称',
  `quantity` int NOT NULL COMMENT '数量',
  `batch_no` varchar(50) DEFAULT NULL COMMENT '批次号',
  `create_by` bigint DEFAULT NULL COMMENT '创建者',
  `create_time` datetime DEFAULT NULL COMMENT '创建时间',
  `update_by` bigint DEFAULT NULL COMMENT '更新者',
  `update_time` datetime DEFAULT NULL COMMENT '更新时间',
  `remark` varchar(500) DEFAULT NULL COMMENT '备注',
  PRIMARY KEY (`id`),
  KEY `idx_order_id` (`order_id`),
  KEY `idx_sku_id` (`sku_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='移库单明细表';
```

### wms_check_order - 盘点单表
```sql
CREATE TABLE `wms_check_order` (
  `id` bigint NOT NULL AUTO_INCREMENT COMMENT '主键ID',
  `order_no` varchar(50) NOT NULL COMMENT '盘点单号',
  `warehouse_id` bigint NOT NULL COMMENT '仓库ID',
  `order_status` int DEFAULT 0 COMMENT '单据状态（0暂存 1已审核 2已完成 4已作废）',
  `check_type` char(1) DEFAULT '1' COMMENT '盘点类型（1全面盘点 2部分盘点）',
  `check_date` date DEFAULT NULL COMMENT '盘点日期',
  `operator_id` bigint DEFAULT NULL COMMENT '操作员ID',
  `operator_name` varchar(50) DEFAULT NULL COMMENT '操作员姓名',
  `audit_by` bigint DEFAULT NULL COMMENT '审核人ID',
  `audit_time` datetime DEFAULT NULL COMMENT '审核时间',
  `create_by` bigint DEFAULT NULL COMMENT '创建者',
  `create_time` datetime DEFAULT NULL COMMENT '创建时间',
  `update_by` bigint DEFAULT NULL COMMENT '更新者',
  `update_time` datetime DEFAULT NULL COMMENT '更新时间',
  `remark` varchar(500) DEFAULT NULL COMMENT '备注',
  PRIMARY KEY (`id`),
  UNIQUE KEY `uk_order_no` (`order_no`),
  KEY `idx_warehouse_id` (`warehouse_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='盘点单表';
```

### wms_check_order_detail - 盘点单明细表
```sql
CREATE TABLE `wms_check_order_detail` (
  `id` bigint NOT NULL AUTO_INCREMENT COMMENT '主键ID',
  `order_id` bigint NOT NULL COMMENT '盘点单ID',
  `sku_id` bigint NOT NULL COMMENT 'SKU ID',
  `sku_code` varchar(50) DEFAULT NULL COMMENT 'SKU编码',
  `sku_name` varchar(200) DEFAULT NULL COMMENT 'SKU名称',
  `system_qty` int DEFAULT 0 COMMENT '系统数量',
  `check_qty` int DEFAULT 0 COMMENT '盘点数量',
  `diff_qty` int DEFAULT 0 COMMENT '差异数量',
  `price` decimal(10,2) DEFAULT NULL COMMENT '单价',
  `diff_amount` decimal(15,2) DEFAULT 0.00 COMMENT '差异金额',
  `batch_no` varchar(50) DEFAULT NULL COMMENT '批次号',
  `create_by` bigint DEFAULT NULL COMMENT '创建者',
  `create_time` datetime DEFAULT NULL COMMENT '创建时间',
  `update_by` bigint DEFAULT NULL COMMENT '更新者',
  `update_time` datetime DEFAULT NULL COMMENT '更新时间',
  `remark` varchar(500) DEFAULT NULL COMMENT '备注',
  PRIMARY KEY (`id`),
  KEY `idx_order_id` (`order_id`),
  KEY `idx_sku_id` (`sku_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='盘点单明细表';
```

### wms_inventory - 库存表
```sql
CREATE TABLE `wms_inventory` (
  `id` bigint NOT NULL AUTO_INCREMENT COMMENT '主键ID',
  `warehouse_id` bigint NOT NULL COMMENT '仓库ID',
  `sku_id` bigint NOT NULL COMMENT 'SKU ID',
  `quantity` int DEFAULT 0 COMMENT '库存数量',
  `lock_quantity` int DEFAULT 0 COMMENT '锁定数量',
  `available_quantity` int DEFAULT 0 COMMENT '可用数量',
  `create_by` bigint DEFAULT NULL COMMENT '创建者',
  `create_time` datetime DEFAULT NULL COMMENT '创建时间',
  `update_by` bigint DEFAULT NULL COMMENT '更新者',
  `update_time` datetime DEFAULT NULL COMMENT '更新时间',
  PRIMARY KEY (`id`),
  UNIQUE KEY `uk_warehouse_sku` (`warehouse_id`, `sku_id`),
  KEY `idx_sku_id` (`sku_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='库存表';
```

### wms_inventory_history - 库存历史表
```sql
CREATE TABLE `wms_inventory_history` (
  `id` bigint NOT NULL AUTO_INCREMENT COMMENT '主键ID',
  `warehouse_id` bigint NOT NULL COMMENT '仓库ID',
  `sku_id` bigint NOT NULL COMMENT 'SKU ID',
  `quantity` int DEFAULT 0 COMMENT '数量',
  `operation_type` int NOT NULL COMMENT '操作类型（1入库 2出库 3移库 4盘点）',
  `order_id` bigint DEFAULT NULL COMMENT '关联单据ID',
  `order_type` int DEFAULT NULL COMMENT '单据类型',
  `order_no` varchar(50) DEFAULT NULL COMMENT '单据号',
  `batch_no` varchar(50) DEFAULT NULL COMMENT '批次号',
  `before_qty` int DEFAULT 0 COMMENT '变动前数量',
  `after_qty` int DEFAULT 0 COMMENT '变动后数量',
  `operator_id` bigint DEFAULT NULL COMMENT '操作员ID',
  `operator_name` varchar(50) DEFAULT NULL COMMENT '操作员姓名',
  `create_time` datetime DEFAULT NULL COMMENT '创建时间',
  `remark` varchar(500) DEFAULT NULL COMMENT '备注',
  PRIMARY KEY (`id`),
  KEY `idx_warehouse_id` (`warehouse_id`),
  KEY `idx_sku_id` (`sku_id`),
  KEY `idx_order_id` (`order_id`),
  KEY `idx_operation_type` (`operation_type`),
  KEY `idx_create_time` (`create_time`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='库存历史表';
```

---

## 📊 字典数据

### 订单状态字典
| 字典类型 | 字典值 | 字典标签 | 说明 |
|----------|--------|----------|------|
| order_status | 0 | 暂存 | 单据创建未提交 |
| order_status | 1 | 已审核 | 单据已审核 |
| order_status | 2 | 部分完成 | 单据部分完成 |
| order_status | 3 | 已完成 | 单据已完成 |
| order_status | 4 | 已作废 | 单据已作废 |

### 操作类型字典
| 字典类型 | 字典值 | 字典标签 | 说明 |
|----------|--------|----------|------|
| opt_type | 1 | 采购入库 | 采购入库 |
| opt_type | 2 | 外协入库 | 外协入库 |
| opt_type | 3 | 退货入库 | 退货入库 |
| opt_type | 4 | 销售出库 | 销售出库 |
| opt_type | 5 | 外协出库 | 外协出库 |
| opt_type | 6 | 调拨出库 | 调拨出库 |

### 库存操作类型字典
| 字典类型 | 字典值 | 字典标签 | 说明 |
|----------|--------|----------|------|
| inventory_opt | 1 | 入库 | 库存增加 |
| inventory_opt | 2 | 出库 | 库存减少 |
| inventory_opt | 3 | 移库 | 库存转移 |
| inventory_opt | 4 | 盘点 | 库存调整 |

---

## 🔗 索引说明

### 主键索引
所有表都有主键索引，通常为 `id` 字段。

### 唯一索引
- `wms_warehouse`: `uk_warehouse_code` (仓库编码)
- `wms_merchant`: `uk_merchant_code` (商户编码)
- `wms_item_category`: `uk_category_code` (分类编码)
- `wms_item_brand`: `uk_brand_code` (品牌编码)
- `wms_item`: `uk_item_code` (物料编码)
- `wms_item_sku`: `uk_sku_code` (SKU编码)
- `wms_receipt_order`: `uk_order_no` (入库单号)
- `wms_shipment_order`: `uk_order_no` (出库单号)
- `wms_movement_order`: `uk_order_no` (移库单号)
- `wms_check_order`: `uk_order_no` (盘点单号)
- `wms_inventory`: `uk_warehouse_sku` (仓库+SKU)

### 普通索引
- 所有订单表都有 `merchant_id` 和 `warehouse_id` 索引
- 所有订单明细表都有 `order_id` 和 `sku_id` 索引
- `wms_inventory_history` 有 `operation_type` 和 `create_time` 索引

---

## 📝 数据初始化

### 默认管理员账号
```sql
-- 用户名：admin
-- 密码：admin123
-- 密码已加密存储
```

### 默认系统数据
- 系统角色：admin（管理员）、common（普通用户）
- 系统菜单：完整的系统菜单结构
- 系统参数：系统运行参数
- 系统字典：系统通用字典

---

## 🔧 维护建议

1. **定期备份**：建议每天备份数据库
2. **索引优化**：定期分析和优化索引
3. **数据清理**：定期清理历史数据
4. **性能监控**：监控慢查询和性能瓶颈
5. **安全审计**：定期审计用户权限和操作日志

---

*最后更新时间：2026-04-08*
