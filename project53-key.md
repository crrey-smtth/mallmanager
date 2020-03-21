### Vue项目-重点

cd D:\dev_code\qian_duan\vue入门\07-代码\mallmanager53

cd D:\dev_code\qian_duan\vue入门\后端项目\工作空间\vueShop-api-server

### day-07-重点

#### 01-项目-准备-项目目录说明
    1.src
    2.build/ webpack相关代码
    3.config/ 本地服务器配置
    4. .eslintignore eslint排除文件
    5. .eslinttrc eslint配置文件

#### 02-项目-准备-代码规范-自定义指令-lintfix

    1.结尾没有；
    2.必须用全等
    3.不允许出现未使用的变量
    4.不允许出现2行以上的空行

    >在package.json中的自定义指令：指令很长
    >npm run 自定义指令名（dev）
    >npm run lintfix(自动按张规范修正全部的js代码)

#### 03-项目-准备-element-ui-文档分析

#### 04-项目-准备-element-ui-安装-引入

#### 05-项目-准备-项目模板简化-调整

    删除模板中用不到的文件

#### 06-项目-准备-git版本控制

    git/svn
    1.git init -> .git
    2.git status
    3.git add
    4.git commit -m "魏海涛"
    5.在代码托管平台（github）新建远程仓库
    6.git remote add origin https://github.com/crrey-smtth/mallmanager.git
    6.git push -u origin master

#### 07-项目-登录-新建分支-login组件-配置路由

    新建一个分支 专门写登录功能
    git branch  查看有哪些分支
    git checkout -b 分支名 （新建，并切换至新分支）
    新建组件，配置路由

    注意：
        1.commit 每完成一个小功能就commit一次
        2.push操作master去完成
#### 08-项目-登录-引入表单组件

    >el-form
        1.引入
        2.调整标签（h2+el-button）
    >:label-position="top"

#### 09-项目-登录-样式调整

    height:100%
    注意：div#app  height:100%

#### 10-项目-登录-axios插件

    axios不是vue插件 Vue.use(axios)

```js
    import axios from 'axios'
    const MyHttpServer = {}

    MyHttpServer.install = function (Vue, options) {
    // 4. 添加实例方法
     Vue.prototype.$http = axios
}
export default MyHttpServer
```
    在main.js中 导入 之后 Vue.use(插件名)
    结果：this.$http.get()

#### 11-项目-登录-发送登录请求

#### 12-项目-登录-引入提示框组件
    this.$message.success(msg)

#### 13-项目-登录-登录-登录成功-进入home组件

    登录成功-》来到home组件
    1.js编程式导航this.$router.push({name :'home'})
    2.App.vue router-view
    3.新建hom组件
    4.路由index.js 配置路由

#### 14-项目-登录-简化登录请求代码-async和await

```js
// 希望 让异步代码操作看起来像同步代码操作一样
      //1.找到异步操作有结果的代码 前面加await同时接口一步操作的结果res
      //2.找到距离异步操作有结果的代码最近的方法 前面加async
      // ES7   async+await
      const res = await this.$http.post('login', this.formdata)
      const {
        data,
        meta: {msg, status}
      } = res.data

      if (status === 200) {
        this.$message.success(msg + data)
         this.$router.push({name: 'home'})
      } else {
        this.$message.success(msg + data)
      }
```

#### 15-项目-登录-保存taken值

    目的:如果用户没有登录->URL直接来到home组件
    在登录成功时 保存正确的用户token

```js
   localStorage.setItem('token',data.token);
```

#### 16-项目-首页-布局容器-使用-样式调整

    layout布局
    行el-row
    列el-col
    注意24列

#### 17-项目-首页-头部-样式调整

#### 18-项目-首页-侧边栏-导航组件-文档

    el->menu
    1.router 开启路由模式 true index值=path值
    2.unique-opened 是否只保持一个子菜单的展开

#### 19-项目-首页-侧边栏-引入导航组件-调整

    调整el-menu
    index值不能一样

#### 20-项目-首页-进入首页的权限验证

    如果没有登录过 就不能进入home组件

```js
beforeCreate() {
    //获取token
    const token = localStorage.getItem("token");
    if (!token) {
      //token 没有 -> 登录
      this.$router.push({ name: "login" });
    }
    //if token 有  -> 继续渲染组件
    //token 没有 -> 登录
    //newVue之间自动触发
  }
```

#### 21-项目-首页-头部-退出功能
```js
handleSigout() {
      //1.清除token
      localStorage.clear();
      //2.提示
      this.$message.success("用户退出");
      //3.来到Login组件
      this.$router.push({ name: "login" });
    }
```

#### 22-项目-首页-合并分支-新建用户分支
    1.切换 master
    2.git merge dev-login 合并分支
    3.push
    4.新建 dev-user

#### 23-项目-首页-用户管理-用户列表-新建组件-路由配置

    1.home.vue 开启路由模式 index值-》path值
    2.home.vue main->router-view
    3.新建users.vue
    4.router/index.js 在home中children配置users的路由

### day-08重点

#### 01-项目-用户管理-用户列表-面包屑和搜索框

    1.el-card 卡片 小容器
    2.面包屑
    3.el-row>el-col>input+button
    4.调整样式
#### 02-项目-用户管理-用户列表-引入表格组件

    1.el-table(data数据源[]) >el-table-column(lable表头/prop="数据") >字符串数据

```js
<el-table
      :data="tableData"
      style="width: 100%">
      <el-table-column
        prop="date"
        label="日期"
        width="180">
      </el-table-column>
      <el-table-column
        prop="name"
        label="姓名"
        width="180">
      </el-table-column>
      <el-table-column
        prop="address"
        label="地址">
      </el-table-column>
    </el-table>
```

#### 03-项目-用户管理-用户列表-表头处理

    按照效果 调整了表头的数量和label
    type="index" -> 该列的每个单元格的内容从1开始的序号


#### 04-项目-用户管理-用户列表-请求数据-设置请求头

    1.created(){this.getUserList()}
    2.methods:{geiUserList(){发送请求}}
    3.接口文档中 除了登录之外的所有请求都需要进行授权->设置请求头
    4.找axios中关于请求头设置的代码
```js
     const AUTH_TOKEN = localStorage.getItem('token')
     this.$http.defaults.headers.common['Authorization'] = AUTH_TOKEN
```
    5.发送请求

#### 05-项目-用户管理-用户列表-渲染数据-一般数据

    1.结构固执 给this.userlist = res.data.data.users
    2.prop=""赋值

#### 06-项目-用户管理-用户列表-渲染数据-日期格式处理

    1.main.js 全局过滤器 fmtdate
    2.  
    2.1 prop="create_time | fmtdate" 不行
    2.2单元格中的内容只能显示文本 
```html
<!-- <el-table-column prop="create_time | fmtdate" label="创建日期"></el-table-column> -->
```
    2.3 需要给该内容外层加容器template
    不同组件的作用域不能共享数据

```html
<template >{{create_time | fmtdate}}</template>
```
    2.4最终写法
    
    slot-scope自动取上一级找最外层标签el-table所绑定的数据userlist
    slot-scope="scope" 此时 “scope”==userlist数组
    scope.row是数组的每个对象
    scope.row.create_time我们要用的数据

```html
 <!-- 如果单元格内显示的内容不是字符串（文本）
        需要给被显示的内容外层包裹一个template-->

        <!-- template 内部要用数据 设置slot-scope属性
        该属性的值是要用数据create_time的数据源userlist-->

        <!-- slot-scope 的值userlist其实就是el-table 绑定的数据userlist
        userlist.row->数组中的每个对象-->
        <template slot-scope="userlist">{{userlist.row.create_time | fmtdate}}</template>
```

#### 07-项目-用户管理-用户列表-渲染数据-用户状态开关
 el-swich v-model="bool"

```html
    <el-table-column  label="用户状态">
        <template slot-scope="scope">
          <el-switch v-model="scope.row.mg_state" active-color="#13ce66" inactive-color="#ff4949"></el-switch>
        </template>
      </el-table-column>

```
#### 08-项目-用户管理-用户列表-渲染数据-操作

    操作里面不是字符串
    template 容器 slot-scope="scope"
    el-buttton
    size="small" pain
 
#### 09
+-项目-用户管理-用户列表

