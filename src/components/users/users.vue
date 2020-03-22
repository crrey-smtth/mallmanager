<template>
  <el-card class="box-card">
    <!-- 1.面包屑 -->
    <!-- /首页/用户管理/用户列表 -->
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item>首页</el-breadcrumb-item>
      <el-breadcrumb-item>用户管理</el-breadcrumb-item>
      <el-breadcrumb-item>用户列表</el-breadcrumb-item>
    </el-breadcrumb>
    <!-- 2.搜索 -->
    <el-row class="searchRow">
      <el-col>
        <el-input
          @clear="loadUserList()"
          clearable
          placeholder="请输入内容"
          v-model="query"
          class="inputsearch"
        >
          <el-button @click="searchUser()" slot="append" icon="el-icon-search"></el-button>
        </el-input>
        <el-button type="success" @click="showAddUserDia()">添加用户</el-button>
      </el-col>
    </el-row>
    <!-- 3.表格 -->
    <el-table :data="userlist" style="width: 100%">
      <el-table-column type="index" label="#" width="60"></el-table-column>
      <el-table-column prop="username" label="姓名" width="80"></el-table-column>
      <el-table-column prop="email" label="邮箱"></el-table-column>
      <el-table-column prop="mobile" label="电话"></el-table-column>
      <!-- {{create_time | fmtdate}} -->
      <!-- <el-table-column prop="create_time | fmtdate" label="创建日期"></el-table-column> -->

      <el-table-column label="创建日期">
        <!-- 如果单元格内显示的内容不是字符串（文本）
        需要给被显示的内容外层包裹一个template-->

        <!-- template 内部要用数据 设置slot-scope属性
        该属性的值是要用数据create_time的数据源userlist-->

        <!-- slot-scope 的值userlist其实就是el-table 绑定的数据userlist
        userlist.row->数组中的每个对象-->
        <template slot-scope="userlist">{{userlist.row.create_time | fmtdate}}</template>
      </el-table-column>

      <el-table-column label="用户状态">
        <template slot-scope="scope">
          <el-switch
            @change="changMgState(scope.row)"
            v-model="scope.row.mg_state"
            active-color="#13ce66"
            inactive-color="#ff4949"
          ></el-switch>
        </template>
      </el-table-column>
      <el-table-column prop="address" label="操作">
        <template slot-scope="scope">
          <el-button
            type="primary"
            size="small"
            plain
            icon="el-icon-edit"
            circle
            @click="showEditUserDia(scope.row)"
          ></el-button>
          <el-button
            type="danger"
            size="small"
            plain
            icon="el-icon-delete"
            @click="showDeleUserMsgBox(scope.row.id)"
            circle
          ></el-button>
          <el-button
            @click="showSetUserRoleDia(scope.row)"
            type="success"
            size="small"
            plain
            icon="el-icon-check"
            circle
          ></el-button>
        </template>
      </el-table-column>
    </el-table>
    <!-- 4.分页 -->
    <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="pagenum"
      :page-sizes="[2, 4, 6, 8]"
      :page-size="total"
      layout="total, sizes, prev, pager, next, jumper"
      :total="total"
    ></el-pagination>

    <!-- 对话框 -->
    <!-- 添加用户的对话框 -->
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
        <el-button type="primary" @click="addUser()">确 定</el-button>
      </div>
    </el-dialog>
    <!-- 更新用户的对话框 -->
    <el-dialog title="编辑用户" :visible.sync="dialogFormVisibleEdit">
      <el-form :model="form">
        <el-form-item label="用户名" :label-width="formLabelWidth">
          <el-input disabled v-model="form.username" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="邮 箱" :label-width="formLabelWidth">
          <el-input v-model="form.email" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="电 话" :label-width="formLabelWidth">
          <el-input v-model="form.mobile" autocomplete="off"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisibleEdit = false">取 消</el-button>
        <el-button type="primary" @click="editUser()">确 定</el-button>
      </div>
    </el-dialog>
    <!-- 分配角色的对话框 -->
    <el-dialog title="分配角色" :visible.sync="dialogFormVisibleRol">
      <el-form :model="form">
        <el-form-item label="用户名" label-width="100px">{{currUsername}}</el-form-item>
        <el-form-item label="角色" label-width="100px">
          <!-- 如果select的绑定的数据的值 和 option 的值一样，
                就会显示该option的label值
          -->
          <el-select v-model="currRoleId">
            <el-option label="请选择" :value="-1"></el-option>
            <!-- <el-option label="区域二" value="beijing"></el-option> -->
            <el-option :label="item.roleName" :value="item.id" v-for="(item,i) in roles" :key="i"></el-option>
          </el-select>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisibleRol = false">取 消</el-button>
        <el-button type="primary" @click="setRole()">确 定</el-button>
      </div>
    </el-dialog>
  </el-card>
</template>
<script>
export default {
  data() {
    return {
      query: "",
      // 表格数据
      userlist: [],
      // 分页相关
      total: -1,
      pagenum: 1,
      pagesize: 2,
      // 添加对话框的属性
      dialogFormVisibleAdd: false,
      // 编辑对话框的属性
      dialogFormVisibleEdit: false,
      // 分配角色对话框
      dialogFormVisibleRol: false,
      // 添加用户的表单数据
      form: {
        username: "",
        password: "",
        email: "",
        mobile: ""
      },
      currRoleId: -1,
      // currUserId:-1,
      formLabelWidth: "100px",
      currUsername: "",
      roles: [],
      currUserId:-1,
    };
  },
  created() {
    this.getUserList();
  },
  methods: {
    //分配角色 修改角色
   async setRole() {
      //users/:id/role
      //:id 要修改的用户的id 值
      //请求体中rid 修改的新值角色id
      const res = await this.$http.put(`users/${this.currUserId}/role`,{
        rid:this.currRoleId
      })
      console.log(res);
      this.dialogFormVisibleRol =false;
      
    },
    // 显示角色对话框
    async showSetUserRoleDia(user) {
      this.currUsername = user.username;
      //
      this.currUserId = user.id;
      // 获取所有角色
      const res1 = await this.$http.get(`roles`);
      console.log(res1);
      this.roles = res1.data.data;
      // 获取当前用户的角色id ->rid
      const res = await this.$http.get(`users/${user.id}`);

      //console.log(res)
      this.currRoleId = res.data.data.rid;
      this.dialogFormVisibleRol = true;
    },
    // 修改状态
    async changMgState(user) {
      const res = await this.$http.put(
        `users/${user.id}/state/${user.mf_state}`,
        this.form
      );
      console.log(res);
    },
    // 编辑用户  发送请求
    async editUser() {
      const res = await this.$http.put(`users/${this.form.id}`, this.form);
      // console.log(res)

      // 1.关闭对话框
      this.dialogFormVisibleEdit = false;
      // 2.更新视图
      this.getUserList();
    },
    // 编辑用户，显示对话框
    showEditUserDia(user) {
      this.dialogFormVisibleEdit = true;
      this.form = user;
    },
    // 删除用户 -打开消息盒子
    showDeleUserMsgBox(userid) {
      this.$confirm("刪除用戶?", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning",
        center: true
      })
        .then(async () => {
          // 发送删除请求 ：id  用户id
          // 1.data中找userid
          // 2.把userId以参数形式传进来
          const res = await this.$http.delete(`users/${userid}`);
          // console.log(res)
          if (res.data.meta.status === 200) {
            // 更新视图
            this.getUserList();
            // 提示
            this.$message({
              type: "success",
              message: "删除成功!"
            });
          }
        })
        .catch(() => {
          this.$message({
            type: "info",
            message: "已取消删除"
          });
        });
    },
    // 发送添加用户请求
    async addUser() {
      // 2.关闭对话框
      this.dialogFormVisibleAdd = false;
      const res = await this.$http.post(`users`, this.form);
      // console.log(res)
      const {
        meta: { status, msg }
      } = res.data;
      if (status === 201) {
        // 1. 提示成功
        this.$message.success(msg);

        // 3.更新视图
        this.getUserList();

        // 4.清空文本框
        // this.form = {}
        for (const key in this.form) {
          if (this.form.hasOwnProperty(key)) {
            this.form[key] = "";
          }
        }
      } else {
        this.$message.warning(msg);
      }
    },
    // 打开添加对话框
    showAddUserDia() {
      this.dialogFormVisibleAdd = true;
    },
    // 清空搜索框，重新加载所有数据
    loadUserList() {
      this.getUserList();
    },
    // 搜索用户
    searchUser() {
      // 按照input绑定的值
      this.getUserList();
    },
    // 分页相关方法
    handleSizeChange(val) {
      console.log(`每页 ${val} 条`);
      this.pagesize = val;
      // this.pagenum=1
      this.getUserList();
    },
    handleCurrentChange(val) {
      console.log(`当前页: ${val}`);
      this.pagenum = val;
      this.getUserList();
    },

    // 获取用户列表的请求
    async getUserList() {
      // query 	查询参数 	可以为空
      // pagenum 	当前页码 	不能为空
      // pagesize 	每页显示条数 	不能为空

      // 需要授权的 API ，必须在请求头中使用 `Authorization` 字段提供 `token` 令牌
      const AUTH_TOKEN = localStorage.getItem("token");
      this.$http.defaults.headers.common["Authorization"] = AUTH_TOKEN;
      const res = await this.$http.get(
        `users?query=${this.query}&pagenum=${this.pagenum}&pagesize=${this.pagesize}`
      );
      // console.log(res)
      const {
        meta: { status, msg },
        data: { users, total }
      } = res.data;
      if (status === 200) {
        // 1.给表格数据赋值
        this.userlist = users;
        // 2.给total赋值
        this.total = total;
        // 3.提示
        this.$message.success(msg);
      } else {
        this.$message.warning(msg);
      }
    }
  }
};
</script>

<style >
.inputsearch {
  width: 300px;
}
.searchRow {
  margin-top: 20px;
}
</style>
