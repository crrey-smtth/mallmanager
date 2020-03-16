### Vue项目-重点

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

#### 14-项目

#### 15-项目

#### 09-项目
