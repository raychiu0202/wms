<template>
  <div class="app-container home">
    <el-row :gutter="20">
      <el-col :sm="24" :lg="12" style="padding-left: 20px">
        <h2>WMS仓库管理系统</h2>
        <p>
          WMS (Warehouse Management System) 是基于 Spring Boot + Vue 3 开发的现代化仓库管理系统，采用前后端分离架构，支持多仓库、多商户的库存管理。
        </p>
        <p>
          <b>当前版本：</b><span>v{{ version }}</span>
        </p>
        <p>
          <el-tag type="success" effect="dark">多仓库管理</el-tag>
          <el-tag type="success" effect="dark">库存实时管理</el-tag>
          <el-tag type="success" effect="dark">权限控制</el-tag>
        </p>
      </el-col>
      <el-col :sm="24" :lg="12" style="padding-left: 50px">
        <img src="@/assets/images/logo.png" alt="Logo" style="width: 200px; height: 200px;">
      </el-col>
    </el-row>
    <el-divider />
    <el-row :gutter="20">
      <el-col :span="24">
        <h3>功能特性</h3>
        <ul>
          <li>多仓库管理</li>
          <li>入库/出库/移库/盘点</li>
          <li>库存实时管理</li>
          <li>基础数据管理</li>
          <li>权限控制</li>
          <li>数据导入导出</li>
          <li>单据打印功能</li>
        </ul>
      </el-col>
    </el-row>
  </div>
</template>

<script setup name="Index">
import { ref } from 'vue'

const version = ref('1.0.0')
</script>

<style scoped lang="scss">
.home {
  font-family: "open sans", "Helvetica Neue", Helvetica, Arial, sans-serif;
  font-size: 13px;
  color: #676a6c;
  overflow-x: hidden;
  padding: 20px;

  h2 {
    margin-top: 0px;
    margin-bottom: 15px;
    font-size: 24px;
    font-weight: bold;
    color: #676a6c;
  }

  h3 {
    margin-top: 20px;
    margin-bottom: 15px;
    font-size: 18px;
    font-weight: bold;
    color: #676a6c;
  }

  p {
    margin-top: 10px;
    line-height: 1.8;
  }

  ul {
    margin-top: 10px;
    padding-left: 20px;

    li {
      margin: 8px 0;
      line-height: 1.6;
    }
  }
}
</style>
