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
                    <el-input placeholder="请输入内容" v-model="query" class="input-search" clearable @clear="loadUserList()">
                        <el-button slot="append" icon="el-icon-search" @click="searchUser()"></el-button>
                    </el-input>
                    <el-button type="success" @click="dialogFormVisibleAdd = true">添加用户</el-button>
                </div>
            </el-col>
        </el-row>
        <!-- 表格 -->
        <el-table :data="userList" style="width: 100%" class="table">
            <el-table-column prop label="序号" type="index"></el-table-column>
            <el-table-column prop="username" label="姓名" width="180"></el-table-column>
            <el-table-column prop="email" label="邮箱"></el-table-column>
            <el-table-column prop="mobile" label="电话"></el-table-column>
            <el-table-column prop label="创建时间">
                <!-- 如果单元格内显示的内容不是字符串（文本），需要给被显示的内容外层包裹一个template -->
                <!-- template内部要用数据  需要设置slot-scope属性
                 该属性的值要用create_time的内部数据源userlist
          -->

                <!-- slot-scope的值userlist其实就是el-table绑定的数据userlist -->
                <!-- userlist.row -> 数组中的每个对象 -->
                <template slot-scope="scope">{{scope.row.create_time | fmtdate}}</template>
            </el-table-column>
            <el-table-column prop label="用户状态">
                <template slot-scope="scope">
                    <el-switch v-model="scope.row.mg_status" active-color="#13ce66" inactive-color="#ff4949"></el-switch>
                </template>
            </el-table-column>
            <el-table-column prop label="操作">
                <template slot-scope>
                    <el-button type="primary" icon="el-icon-edit" circle plain></el-button>
                    <el-button type="success" icon="el-icon-check" circle plain></el-button>
                    <el-button type="danger" icon="el-icon-delete" circle plain></el-button>
                </template>
            </el-table-column>
        </el-table>
        <!-- 分页 -->
        <!-- current-page设置当前页是第几页
             page-sizes每页显示多少条数据的数组
             page-size="100" 设置初始时显示多少条数据
      -->
        <el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="pagenum" :page-sizes="[2, 4, 6]" :page-size="2" layout="total, sizes, prev, pager, next, jumper" :total="total"></el-pagination>

        <!-- 添加用户弹框 -->
        <el-dialog title="添加用户" :visible.sync="dialogFormVisibleAdd">
            <el-form :model="form">
                <el-form-item label="用户名" :label-width="formLabelWidth">
                    <el-input v-model="form.username" autocomplete="off"></el-input>
                </el-form-item>

                <el-form-item label="密 码" :label-width="formLabelWidth">
                    <el-input v-model="form.password" autocomplete="off"></el-input>
                </el-form-item>

                <el-form-item label="邮 箱" :label-width="formLabelWidth">
                    <el-input v-model="form.email" autocomplete="off"></el-input>
                </el-form-item>

                <el-form-item label="电 话" :label-width="formLabelWidth">
                    <el-input v-model="form.mobile" autocomplete="off"></el-input>
                </el-form-item>
            </el-form>
            <div slot="footer" class="dialog-footer">
                <el-button @click="dialogFormVisibleAdd = false">取 消</el-button>
                <el-button type="primary" @click="adduser()">确 定</el-button>
            </div>
        </el-dialog>
    </el-card>
</div>
</template>

<script>
export default {
    data() {
        return {
            query: "",
            userList: [],
            //分页相关数据
            total: 0,
            pagenum: 1, //当前页码
            pagesize: 2, //每页显示条数
            dialogFormVisibleAdd: false, // 控制弹框可见
            form: { //添加用户的表单数据
                username: '',
                password: '',
                email: '',
                mobile: ''
            }
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
            this.$http.defaults.headers.common[
                "Authorization"
            ] = localStorage.getItem("token");
            this.$http
                .get(
                    `users?query=${this.query}&pagenum=${this.pagenum}&pagesize=${this.pagesize}`
                )
                .then(res => {
                    //   console.log(res)
                    const {
                        meta: {
                            status,
                            msg
                        },
                        data: {
                            users,
                            total
                        }
                    } = res.data;
                    if (status === 200) {
                        // 1.给表格数据赋值
                        this.userList = users;
                        // 2.给total赋值
                        this.total = total;
                        // 3.提示用户获取成功
                        // this.$message.success(msg);
                    }
                });
        },
        handleSizeChange(val) {
            //每页显示条数变化时触发
            //   console.log(`每页 ${val} 条`);
            this.pagesize = val;
            this.pagenum = 1;
            this.getUserList();
        },
        handleCurrentChange(val) {
            //当前页改变时触发
            //   console.log(`当前页: ${val}`);
            this.pagenum = val;
            this.getUserList();
        },
        searchUser() {
            this.getUserList();
        },
        // 清空搜索框时重新获取数据
        loadUserList() {
            this.getUserList();
        },

        // 添加用户，发送请求
        adduser() {
          this.$http.post('users',this.form).then(res => {
            // console.log(res)
            const {meta:{status,msg},data} = res.data

            if(status === 201){
              //提示成功，关闭对话框，清空对话框数据  刷新数据
              this.$message.success(msg)
              this.dialogFormVisibleAdd = false
              this.form.username = ''
              this.form.password = ''
              this.form.email = ''
              this.form.mobile = ''

              this.getUserList()
            }else{
              this.$message.error(msg)
            }
          })
        }
    }
};
</script>

<style>
.users {
    height: 100%;
    background-color: pink;
}

.input-search {
    width: 500px;
}

.box-card {
    height: 100%;
}
</style>
