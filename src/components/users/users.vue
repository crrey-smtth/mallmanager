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
        <el-input placeholder="请输入内容" v-model="query" class="inputsearch">
          <el-button slot="append" icon="el-icon-search"></el-button>
        </el-input>
        <el-button type="success">添加用户</el-button>
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
          <el-switch v-model="scope.row.mg_state" active-color="#13ce66" inactive-color="#ff4949"></el-switch>
        </template>
      </el-table-column>
      <el-table-column prop="address" label="操作">
        <template slot-scope="scope">
          <el-button type="primary" size="small" plain icon="el-icon-edit" circle></el-button>
          <el-button type="danger" size="small" plain icon="el-icon-delete" circle></el-button>
          <el-button type="success" size="small" plain icon="el-icon-check" circle></el-button>
        </template>
      </el-table-column>
    </el-table>
  </el-card>
</template>
      
    </el-table>
    <!-- 4.分页 -->
  </el-card>
</template>

<script>
export default {
  data() {
    return {
      query: "",
      // 表格数据
      userlist: [],
      //分页相关
      total: -1,
      pagenum: 1,
      pagesize: 2
    };
  },
  created() {
    this.getUserList();
  },
  methods: {
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
      //console.log(res)
      const {
        meta: { status, msg },
        data: { users, total }
      } = res.data;
      if (status === 200) {
        //1.给表格数据赋值
        this.userlist = users;
        //2.给total赋值
        this.total = total;
        //3.提示
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
