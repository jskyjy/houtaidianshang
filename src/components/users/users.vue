<template>
  <div class="users">
    <el-card class="box-card">
      <!-- 面包屑 -->
      <el-breadcrumb separator-class="el-icon-arrow-right">
        <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
        <el-breadcrumb-item>用户管理</el-breadcrumb-item>
        <el-breadcrumb-item>用户列表</el-breadcrumb-item>
      </el-breadcrumb>
      <!-- 搜索框 -->
      <el-row>
        <el-col>
          <div style="margin-top: 15px;">
            <el-input placeholder="请输入内容" v-model="query" class="input-search">
              <el-button slot="append" icon="el-icon-search"></el-button>
            </el-input>
            <el-button type="success">添加用户</el-button>
          </div>
        </el-col>
      </el-row>
      <!-- 表格 -->
      <el-table :data="users" style="width: 100%">
        <el-table-column prop="date" label="序号" type="index"></el-table-column>
        <el-table-column prop="name" label="姓名" width="180"></el-table-column>
        <el-table-column prop="address" label="邮箱"></el-table-column>
        <el-table-column prop="address" label="电话"></el-table-column>
        <el-table-column prop="address" label="创建时间"></el-table-column>
        <el-table-column prop="address" label="用户状态"></el-table-column>
        <el-table-column prop="address" label="操作"></el-table-column>
      </el-table>
      <!-- 分页 -->
    </el-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      query: "",
      pagenum: 1, //当前页码
      pagesize: 2, //每页显示条数
      users: [{ data: 2019 - 9 - 10, name: "沈淫", adress: "FFFFFFFF" }]
    };
  },
  created() {
    this.getUserList();
  },
  methods: {
    getUserList() {
      // query	    查询参数	可以为空
      // pagenum	    当前页码	不能为空
      // pagesize	每页显示条数	不能为空

      //需要授权的 API ，必须在请求头中使用 Authorization 字段提供 token 令牌
      this.$http.defaults.headers.common["Authorization"] = localStorage.getItem("token");
      this.$http.get(`users?query=${this.query}&pagenum=${this.pagenum}&pagesize=${this.pagesize}`).then(res => {
          console.log(res)
      });
    }
  }
};
</script>

<style>
.users {
  height: 100%;
}
.input-search {
  width: 500px;
}
</style>