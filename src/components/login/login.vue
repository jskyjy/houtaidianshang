<template>
  <div class="login-wrap">
    <el-form label-position="top" label-width="80px" :model="formdata" class="login-form">
      <h2>用户登录</h2>
      <el-form-item label="用户名">
        <el-input v-model="formdata.username"></el-input>
      </el-form-item>
      <el-form-item label="密码">
        <el-input v-model="formdata.password" type="password"></el-input>
      </el-form-item>
      <el-button type="primary" class="login-btn" @click="handleLogin()">登录</el-button>
    </el-form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      formdata: {
        username: "",
        password: ""
      }
    };
  },
  methods: {
    //登录请求
    handleLogin() {
      this.$http.post("login", this.formdata).then(res => { //异步操作代码
        const {
          data,
          meta: { msg, status }
        } = res.data;

        if (status === 200) {
          //登录成功
          // 将token值存储到localstorage中
          localStorage.setItem('token',data.token)
          //跳转home，消息提示
          this.$router.push({ name: "home" });  //跳转到home组件
          this.msg = msg
          this.$message.success(msg)
        }else{
            this.$message.warning(msg)  //element-ui消息弹窗组件
        }
      });
    }
  }
};
</script>


<style>
.login-wrap {
  background-color: #324152;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}
.login-wrap .login-form {
  width: 400px;
  background-color: #fff;
  padding: 30px;
  border-radius: 5px;
}
.login-btn {
  width: 100%;
}
</style>