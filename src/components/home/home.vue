<template>
  <el-container class="container">
    <el-header class="header">
      <el-row>
        <el-col :span="4">
          <div class="grid-content bg-purple">
            <img
              src="../../assets/login.png"
              alt="无法显示图片"
              class="logo"
              style="width:100%,height:100%"
            />
          </div>
        </el-col>
        <el-col :span="18">
          <h2 class="middle">商品管理系统</h2>
        </el-col>
        <el-col :span="2">
          <div class="grid-content bg-purple">
            <a class="loginout" href="#" @click="handleSigout()">退出</a>
          </div>
        </el-col>
      </el-row>
    </el-header>
    <el-container>
      <el-aside class="aside" width="200px">
        <el-menu
        :router="true"
        :unique-opened="true">
          <!--1-->
          <el-submenu index="1">
            <template slot="title">
              <i class="el-icon-location"></i>
              <span>用户管理</span>
            </template>
            <el-menu-item index="users">
              <i class="el-icon-eleme"></i>
              <span>用户列表</span>
            </el-menu-item>
          </el-submenu>
          <!--2-->
          <el-submenu index="2">
            <template slot="title">
              <i class="el-icon-location"></i>
              <span>权限管理</span>
            </template>
            <el-menu-item index="1-1">
              <i class="el-icon-eleme"></i>
              <span>角色列表</span>
            </el-menu-item>
            <el-menu-item index="1-1">
              <i class="el-icon-eleme"></i>
              <span>权限管理</span>
            </el-menu-item>
          </el-submenu>
          <!--3-->
          <el-submenu index="3">
            <template slot="title">
              <i class="el-icon-location"></i>
              <span>商品管理</span>
            </template>
            <el-menu-item index="1-1">
              <i class="el-icon-eleme"></i>
              <span>商品列表</span>
            </el-menu-item>
            <el-menu-item index="1-1">
              <i class="el-icon-eleme"></i>
              <span>分类参数</span>
            </el-menu-item>
            <el-menu-item index="1-1">
              <i class="el-icon-eleme"></i>
              <span>商品分类</span>
            </el-menu-item>
          </el-submenu>
          <!--4-->
          <el-submenu index="4">
            <template slot="title">
              <i class="el-icon-location"></i>
              <span>订单管理</span>
            </template>
            <el-menu-item index="1-1">
              <i class="el-icon-eleme"></i>
              <span>订单列表</span>
            </el-menu-item>
          </el-submenu>
          <!--5-->
          <el-submenu index="5">
            <template slot="title">
              <i class="el-icon-location"></i>
              <span>数据统计</span>
            </template>
            <el-menu-item index="1-1">
              <i class="el-icon-eleme"></i>
              <span>数据报表</span>
            </el-menu-item>
          </el-submenu>
        </el-menu>
      </el-aside>
      <el-main class="main">Main</el-main>
    </el-container>
  </el-container>
</template>

<script>
export default {
  beforeCreate () {
    // 获取token
    const token = localStorage.getItem('token')
    if (!token) {
      // token 没有 -> 登录
      this.$router.push({ name: 'login' })
    }
    // if token 有  -> 继续渲染组件
    // token 没有 -> 登录
    // newVue之间自动触发
  },
  methods: {
    handleSigout () {
      // 1.清除token
      localStorage.clear()
      // 2.提示
      this.$message.success('用户退出')
      // 3.来到Login组件
      this.$router.push({ name: 'login' })
    }
  }
}
</script>

<style >
.container {
  height: 100%;
}

.header {
  background-color: #b3c0d1;
}

.aside {
  background-color: #d3dce6;
}

.main {
  background-color: #e9eef3;
}
.middle {
  text-align: center;
}
.loginout {
  line-height: 60px;
  text-decoration: none;
}
.logo {
  height: 100%;
  width: 100%;
}
</style>
