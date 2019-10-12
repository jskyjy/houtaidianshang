<template>
  <div class="home">
    <el-container class="container">
      <!-- header -->
      <el-header class="header">
        <el-row>
          <el-col :span="4">
            <div class="grid-content bg-purple">
              <img src="../../assets/images/heima_logo.png" alt />
            </div>
          </el-col>
          <el-col :span="18">
            <div class="grid-content bg-purple-light">
              <h2>电商后台管理项目</h2>
            </div>
          </el-col>
          <el-col :span="2">
            <div class="grid-content bg-purple">
              <a href="#" @click.prevent = "handleSignout()">退出</a>
            </div>
          </el-col>
        </el-row>
      </el-header>
      <el-container>
        <!-- aside -->
        <el-aside width="200px" class="aside">
          <el-menu 
          :unique-opened="true"
          :router="true"
          >
            <!-- 1 -->
            <el-submenu index="1">
              <template slot="title">
                <i class="el-icon-user"></i>
                <span>用户管理</span>
              </template>
              <el-menu-item index="/users">
                <i class="el-icon-notebook-1"></i>
                <span>用户列表</span>
              </el-menu-item>
            </el-submenu>
            <!-- 2 -->
            <el-submenu index="2">
              <template slot="title">
                <i class="el-icon-lock"></i>
                <span>权限管理</span>
              </template>
              <el-menu-item index="2-1">
                <i class="el-icon-menu"></i>
                <span>角色列表</span>
              </el-menu-item>
              <el-menu-item index="2-2">
                <i class="el-icon-menu"></i>
                <span>权限列表</span>
              </el-menu-item>
            </el-submenu>
            <!-- 3 -->
            <el-submenu index="3">
              <template slot="title">
                <i class="el-icon-shopping-bag-1"></i>
                <span>商品管理</span>
              </template>
              <el-menu-item index="3-1">
                <i class="el-icon-menu"></i>
                <span>商品列表</span>
              </el-menu-item>
              <el-menu-item index="3-2">
                <i class="el-icon-menu"></i>
                <span>分类参数</span>
              </el-menu-item>
              <el-menu-item index="3-3">
                <i class="el-icon-menu"></i>
                <span>商品分类</span>
              </el-menu-item>
            </el-submenu>
            <!-- 4 -->
            <el-submenu index="4">
              <template slot="title">
                <i class="el-icon-s-help"></i>
                <span>订单管理</span>
              </template>
              <el-menu-item index="4-1">
                <i class="el-icon-menu"></i>
                <span>订单列表</span>
              </el-menu-item>
            </el-submenu>
            <!-- 5 -->
            <el-submenu index="5">
              <template slot="title">
                <i class="el-icon-s-data"></i>
                <span>数据统计</span>
              </template>
              <el-menu-item index="5-1">
                <i class="el-icon-menu"></i>
                <span>数据报表</span>
              </el-menu-item>
            </el-submenu>
          </el-menu>
        </el-aside>
        <!-- main -->
        <el-main class="main">
          <router-view></router-view>
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>

<script>
export default {
  //进去home页面渲染之前应该查看是都存在token值，若存在继续渲染，不存在跳转回登录页面。我们选择在beforecreate里面
  beforeCreate() {
    const token = localStorage.getItem('token')
    if(!token){
      this.$router.push({name:'login'})
    }
  },
  data(){
    return {

    }
  },
  methods:{
    // 退出功能，清楚token，回到login组件
    handleSignout(){
      localStorage.removeItem('token')
      this.$message.success('退出成功')
      this.$router.push({name:'login'})

    }
  }
};
</script>

<style>
.home {
  height: 100%;
}
.container {
  height: 100%;
}
.header {
  background-color: #b3c0d1;
}
.header h2 {
  text-align: center;
  line-height: 62px;
}
.header a {
  text-decoration: none;
  color: black;
  line-height: 62px;
}
.aside {
  background-color: #d3dce6;
}
.main {
  background-color: #e9eef3;
}
</style>