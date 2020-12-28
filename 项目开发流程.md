

# 一.建库建表

```
1.开启mysql服务
2.打开数据库(navcat)
3.创建库名 shop0824
4.打开dos,  mysql -uroot -p
5.use shop0824  使用数据库
6.source 文件路径
7.show tables;
```

![image-20201217092205250](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201217092205250.png)

# 二.打开后端服务

```js
1.在shop-admin 修改连接数据库的配置:   config/global.js
	// 数据库连接参数
        exports.dbConfig = {
            host: 'localhost', //数据库地址
            user: 'root',//数据库用户名
            password: '123456',//数据库用户密码
            port: 3306,
            database: 'shop0824' // 数据库名字
        }
2.npm i  安装依赖项
3.npm start  开启后端服务
```



# 三.初始化流程

## 1.项目初始化

```
1.新建目录shop-home
2.初始化vue项目: vue init webpack
3.cnpm i  安装依赖项
4.npm run dev
```

## 2.清空工作

```
1.App.vue  只留vue模板
2.router/index.js  将有关hello的内容删除
3.components  清空
4.assets      清空
5.main.js		唯一入口文件
```



## 3.引入重置样式文件

```js
1.assets/css/reset.css
	*{
      margin: 0;
      padding: 0;
      list-style: none;
      text-decoration: none;
    }

2.在main.js中将reset.css引入
	//1.引入重置的css样式
	import './assets/css/reset.css'
```

## 4.引入全局公共组件

```js
1.components/index.js
	export default {

	}
2.在main.js中引入
	//2.引入全局公共组件
        import commonComponents from './components';
        for(let i in commonComponents){
          Vue.component(i,commonComponents[i])
        }
```

## 5.引入全局过滤器

```js
1.filters/index.js
	export default {

	}
2.在main.js中引入
	//3.引入全局过滤器
    import filters from './filters';
    for(let i in filters){
      Vue.filter(i,filters[i])
    }
```

## 6.数据请求

1.安装

```
cnpm i axios qs --save
```

2.配置代理

```js
1.config/index.js
	proxyTable: {
      "/api":{
        target:"http://localhost:3000",
        changeOrigin:true,//是否跨域
        pathRewrite:{
          "^/api":"http://localhost:3000"
        }
      }
    },
```

3.请求配置文件

```js
1.utils/request.js
	import axios from 'axios';
    import qs from 'qs';
	//基础路径
    const baseUrl = "/api";

    // 响应拦截
    axios.interceptors.response.use(res=>{
      console.group("本次请求的路径为:"+res.config.url)
      console.log(res);
      return res
    })

```

## 7.引入vuex

1.安装

```
cnpm i vuex --save
```

2.配置状态管理层

```js
1.store/index.js
	import Vue from 'vue';
    import Vuex from 'vuex';
    Vue.use(Vuex)

    import {state,mutations,getters} from './mutations.js';
    import actions from './actions.js';
    export default new Vuex.Store({
      state,
      mutations,
      actions,
      getters,
      modules:{}
    })
2.store/mutations.js
	export const state = {}
    export const mutations = {}
    export const getters = {}
3.store/actions.js
	export default {
    
	}
4.store/modules  (空的目录)
5.在main.js引入
	//5.引入store
    import store from './store';

    /* eslint-disable no-new */
    new Vue({
      el: '#app',
      router,
      components: { App },
      template: '<App/>',
      store
    })
```

## 8.element-UI

1.安装

```
cnpm i element-ui --save
```

2.引入

```js
1.在main.js中
	//6.引入elementui
    import ElementUI from 'element-ui';
    import 'element-ui/lib/theme-chalk/index.css';
    Vue.use(ElementUI);
```

## 9.弹框组件

```js
utils/alert.js
	import Vue from 'vue';
    const vm = new Vue()

    // 成功的消息提示
    export const successAlert = (msg)=>{
      vm.$message({
        message: msg,
        type: 'success'
      });
    }


    // 警告消失提示
    export const warningAlert = (msg)=>{
      vm.$message({
        message: msg,
        type: 'warning'
      });
    }

    // 错误提示
    export const errorAlert = (msg)=>{
      vm.$message.error(msg);
    }


```



# 四.项目目录架构

```
-src
	-assets
		css
			reset.css 重置样式
		js
	-components
		index.js  存放公共组件
	-filters
		index.js  存放全局过滤器
	-pages			你的项目
	-router
		index.js	配置路由规则
	-store
		index.js
		mutaions.js
		actions.js
		modules
	-utils
		request.js   发送数据请求文件
		alert.js    消失提示弹框文件
	App.vue				根组件
	main.js			唯一入口文件
	
```

重新启动下项目npm run dev,成功表示已经OK了.

# 五.开发流程

## login.vue

```vue
<template>
  <div class="login">
    <div class="con">
      <h3>登录</h3>
      <el-input placeholder="请输入用户"></el-input>
      <el-input type="password"  placeholder="请输入密码"></el-input>
      <div class="btn">
        <el-button type="primary" @click="login">登录</el-button>
      </div>

    </div>
  </div>
</template>

<script>
export default {
  methods:{
    login(){
      this.$router.push('/')
    }
  }
}
</script>

<style scoped>
.login{
  width:100vw;
  height: 100vh;
  background: linear-gradient(#553443,#2F3D60);
  position: fixed;
  left: 0;
  top:0;
}
.con{
  width:400px;
  height: 300px;
  background: white;
  border-radius: 10px;
  position: absolute;
  left:50%;
  top:50%;
  transform: translate(-50%,-50%);
}
h3{
  text-align: center;
  margin: 20px;
}
.el-input,.el-button{
  width:90%;
  margin:15px;
}
</style>

```

1.App.vue

```vue
<template>
  <div>
    <!-- 路由出口 -->
    <router-view></router-view>
  </div>
</template>
```

2.配置路由规则

```js
 routes: [
    {
      path:'/login',
      component:()=>import('../pages/login/login.vue')
    },
    {
      path:'/',
      component:()=>import('../pages/index/index.vue')
    },
  ]
```

3.新建一个index组件

## index.vue

```vue
<template>
  <el-container>
  <el-aside width="150px">
    <el-menu
      class="el-menu-vertical-demo"
      background-color="#545c64"
      text-color="#fff"
      active-text-color="#ffd04b" :unique-opened="true" router>
        //如果你设置了router,那么index可以为path路径的跳转
       <el-menu-item index="/">
        <i class="el-icon-menu"></i>
        <span slot="title">首页</span>
      </el-menu-item>
      <el-submenu index="1">
        <template slot="title">
          <i class="el-icon-location"></i>
          <span>系统设置</span>
        </template>
          <el-menu-item index="/menu">菜单管理</el-menu-item>
          <el-menu-item index="/role">角色管理</el-menu-item>
          <el-menu-item index="/manager">管理员管理</el-menu-item>
      </el-submenu>
      <el-submenu index="2">
        <template slot="title">
          <i class="el-icon-location"></i>
          <span>商城管理</span>
        </template>
          <el-menu-item index="/category">商品分类</el-menu-item>
          <el-menu-item index="/specs">商品规格</el-menu-item>
          <el-menu-item index="/goods">商品管理</el-menu-item>
          <el-menu-item index="/member">会员管理</el-menu-item>
          <el-menu-item index="/banner">轮播管理</el-menu-item>
          <el-menu-item index="/seckill">秒杀活动</el-menu-item>
      </el-submenu>
    </el-menu>
  </el-aside>
  <el-container>
    <el-header height="80px">
      <span>admin</span>
      <el-button type="primary">退出</el-button>
    </el-header>
    <el-main>
      <el-breadcrumb separator-class="el-icon-arrow-right">
        <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
        <el-breadcrumb-item>{{$route.name}}</el-breadcrumb-item>
      </el-breadcrumb>
      <router-view></router-view>
      </el-main>
  </el-container>
</el-container>
</template>

<script>
export default {

}
</script>

<style scoped>
.el-aside{
  height: 100vh;
  background: #333;
  color:white;
}
.el-header{
  /* height: 80px; */
  background: #B3C0D1;
  text-align: right;
  line-height: 80px;
}
.el-breadcrumb{
  margin-bottom: 20px;
}
</style>

```

配置路由规则

```js
import Vue from 'vue'
import Router from 'vue-router'


Vue.use(Router)

export default new Router({
  routes: [
    {
      path:'/login',
      component:()=>import('../pages/login/login.vue')
    },
    {
      path:'/',
      component:()=>import('../pages/index/index.vue'),
      children:[
        {
          path:'menu',
          name:'菜单',
          component:()=>import('../pages/menu/menu.vue'),
        },
        {
          path:'home',
          name:'首页',
          component:()=>import('../pages/home/home.vue'),
        },
        {
          path:'',
          redirect:'home',
        }
      ]
    },
  ]
})

```

## menu.vue

```vue
<template>
  <div>
    <el-button type="primary" @click="add">添加</el-button>
    <!-- 添加子组件 -->
    <v-add :info="info" ref="add"></v-add>

    <!-- 列表子组件 -->
    <v-list @edit="edit($event)"></v-list>
  </div>
</template>

<script>
import vAdd from './components/add.vue';
import vList from './components/list.vue';
export default {
  data(){
    return {
      info:{
        show:false,
        title:'',
        isAdd:true
      }
    }
  },
  components:{
    vAdd,
    vList
  },
  methods:{
    add(){
      this.info.show = true;
      this.info.title = '添加菜单';
      this.info.isAdd = true
    },
    edit(id){
       this.info.show = true;
       this.info.title = '编辑菜单';
       this.info.isAdd = false;
       this.$refs.add.getDetial(id)
    }
  }
}
</script>

<style scoped>
.el-button{
  margin-bottom: 20px;
}


</style>

```

总结:

```
menu:
1.一个添加按钮
2.一个add子组件
3.一个list子组件
4.edit()-----list组件中通过自定义事件$emit,
```

## add.vue

```vue
<template>
  <el-dialog :title="info.title" :visible.sync="info.show">
  <el-form :model="form">
    <el-form-item label="菜单名称" :label-width="formLabelWidth">
      <el-input v-model="form.title"></el-input>
    </el-form-item>
    <el-form-item label="上级菜单" :label-width="formLabelWidth">
      <el-select v-model="form.pid">
        <el-option label="--请选择--" value="" disabled></el-option>
        <el-option label="顶级菜单" :value="0"></el-option>
        <!-- 动态数据获取 -->
        <el-option v-for="item in list" :key="item.id" :label="item.title" :value="item.id"></el-option>
      </el-select>
    </el-form-item>
     <el-form-item label="菜单类型" :label-width="formLabelWidth">
       <el-radio v-model="form.type" :label="1">目录</el-radio>
      <el-radio v-model="form.type" :label="2">菜单</el-radio>
    </el-form-item>
     <el-form-item label="菜单图标" :label-width="formLabelWidth" v-if="form.type==1">
      <el-select v-model="form.icon">
        <el-option label="--请选择--" value="" disabled></el-option>
        <el-option v-for="item in icons" :key="item" :value="item"></el-option>
      </el-select>
    </el-form-item>
     <el-form-item label="菜单地址" :label-width="formLabelWidth" v-else>
      <el-select v-model="form.url">
        <el-option label="--请选择--" value="" disabled></el-option>
        <el-option v-for="item in urls" :key="item" :value="item"></el-option>
      </el-select>
    </el-form-item>
     <el-form-item label="菜单状态" :label-width="formLabelWidth">
       <el-switch
            v-model="form.status"
            active-color="blue"
            inactive-color="grey" :active-value="1" :inactive-value="2">
          </el-switch>
    </el-form-item>


  </el-form>
  <div slot="footer" class="dialog-footer">
    <el-button @click="cancel">取 消</el-button>
    <el-button type="primary" @click="confirm" v-if="info.isAdd">确 定</el-button>
    <el-button type="primary" @click="update" v-else>修 改</el-button>
  </div>
</el-dialog>
</template>

<script>
import {requestAddMenu,requestMenuOne,requestUpdateMenu} from '../../../utils/request';
import {successAlert,warningAlert} from '../../../utils/alert';
import {mapGetters,mapActions} from 'vuex';
export default {
  props:['info'],
  data(){
    return{
       form: {
          pid:0,
          title:'',
          icon:'',
          type:1,
          url:'',
          status:1
        },
        icons:[
          'el-icon-s-tools',
          'el-icon-goods',
          'el-icon-picture-outline-round',
          'el-icon-s-home',
          'el-icon-s-shop'
        ],
        urls:[
          'home',
          'menu',
          'role',
          'manager',
          'category',
          'specs',
          'goods'
        ],
        formLabelWidth: '120px'
    }
  },
  computed:{
    ...mapGetters({
      'list':'menu/list'
    })
  },
  methods:{
    ...mapActions({
      "requestMenuList":"menu/listActions"
    }),
    cancel(){
      this.info.show = false;
      this.form = {
          pid:0,
          title:'',
          icon:'',
          type:1,
          url:'',
          status:1
        }
    },
    confirm(){
      requestAddMenu(this.form).then(res=>{
        if(res.data.code == 200){
          successAlert(res.data.msg)
          this.cancel()
           this.requestMenuList()
        }else{
          warningAlert(res.data.msg)
        }
      })

    },
    getDetial(id){
      requestMenuOne({id:id}).then(res=>{
        this.form = res.data.list;
        this.form.id = id
      })
    },
    update(){
      requestUpdateMenu(this.form).then(res=>{
        if(res.data.code == 200){
          successAlert(res.data.msg);
          this.cancel()
          this.requestMenuList()
        }else{
          warningAlert(res.data.msg)
        }
      })
    },
    mounted(){
      this.requestMenuList()
    }
  }
}
</script>

<style>

</style>

```

## list.vue

```vue
<template>
  <div>
    <el-table
    :data="list"
    style="width: 100%;margin-bottom: 20px;"
    row-key="id"
    border
    :tree-props="{children: 'children'}">
    <el-table-column  prop="id" label="菜单编号"  width="180"> </el-table-column>
    <el-table-column  prop="title" label="菜单名称"  width="180"> </el-table-column>
    <el-table-column  prop="icon" label="菜单图标"  width="180"> </el-table-column>
    <el-table-column  prop="url" label="菜单地址"  width="180"> </el-table-column>
    <el-table-column label="状态"  width="180">
       <template slot-scope="scope">
       <el-button type="primary" v-if="scope.row.status == 1">启用</el-button>
       <el-button type="danger" v-else>禁止</el-button>
       </template>
    </el-table-column>
    <el-table-column label="操作"  width="180">
      <template slot-scope="scope">
        <el-button type="primary" @click="edit(scope.row.id)">编辑</el-button>
        <el-button type="danger" @click="del(scope.row.id)">删除</el-button>
      </template>
    </el-table-column>

  </el-table>
  </div>
</template>

<script>
import {requestDelMenu} from '../../../utils/request';
import {successAlert} from '../../../utils/alert';
import {mapGetters,mapActions} from  'vuex';
export default {
  data(){
    return{

    }
  },
  computed:{
    ...mapGetters({
      'list':"menu/list"
    })
  },
  methods:{
    ...mapActions({
      'requestMenuList':'menu/listActions'
    }),
    edit(id){
      this.$emit('edit',id)
    },
    del(id){
      this.$confirm('确定要删除吗?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          requestDelMenu({id:id}).then(res=>{
            if(res.data.code == 200){
              successAlert(res.data.msg);
               this.requestMenuList()
            }
          })
        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消删除'
          });
        });
    }
  },
  mounted(){
    this.requestMenuList()
  }
}
</script>

<style>

</style>

```

store/modules/menu.js

```js
import {requestMenuList} from '../../utils/request';
const state = {
  list:[]
}

const mutations = {
  changeList(state,arr){
    state.list = arr
  }
}

const actions = {
  listActions(context){
    // 发起菜单列表请求
    requestMenuList({istree:true}).then(res=>{
      if(res.data.code == 200){
        context.commit('changeList',res.data.list)
      }
    })
  }
}

const getters = {
  list(state){
    return state.list
  }
}



export default {
  state,
  mutations,
  actions,
  getters,
  namespaced:true
}

```

utils/request.js

```js
import axios from 'axios';
import qs from 'qs';

const baseUrl = "/api";

// 响应拦截
axios.interceptors.response.use(res=>{
  console.group("本次请求的路径为:"+res.config.url)
  console.log(res);
  return res
})

// 添加菜单
export const requestAddMenu = (data)=>{
  return axios({
    method:'post',
    url:baseUrl+'/api/menuadd',
    data:qs.stringify(data)
  })
}


// 菜单列表
export const requestMenuList = (params)=>{
  return axios({
    method:'get',
    url:baseUrl+'/api/menulist',
    params:params
  })
}

// 获取菜单详情
export const requestMenuOne = (params)=>{
  return axios({
    method:'get',
    url:baseUrl+'/api/menuinfo',
    params:params
  })
}

// 修改菜单
export const requestUpdateMenu = (data)=>{
  return axios({
    method:'post',
    url:baseUrl+'/api/menuedit',
    data:qs.stringify(data)
  })
}

// 删除菜单
export const requestDelMenu = (data)=>{
  return axios({
    method:'post',
    url:baseUrl+'/api/menudelete',
    data:data
  })
}

```

# 一.项目开发流程

## 1.角色管理

### role.vue

```vue
<template>
  <div>
    <el-button type="primary" @click="add">添加</el-button>

    <!-- 添加子组件 -->
    <v-add :info="info" ref="add"></v-add>
    <!-- 列表子组件 -->
    <v-list @edit="edit($event)"></v-list>
  </div>
</template>

<script>
import vAdd from './components/add.vue';
import vList from './components/list.vue';
import qs from 'qs';
export default {
  data(){
    return {
      info:{
        show:false,
        title:'',
        isAdd:true
      }
    }
  },
  methods:{
    add(){
      this.info.show = true;
      this.info.title = '添加角色';
      this.info.isAdd = true
    },
    // 通过子组件的自定义事件进行的触发的
    edit(id){
      this.info.show = true;
      this.info.title = '编辑角色';
      this.info.isAdd = false;
      this.$refs.add.getDetail(id)
    }
  },
  components:{
    vAdd,
    vList
  },

  mounted(){
    // 解释JSON和qs的区别
  //   var json = {
  //     user:'xiaowang',
  //     age:20,
  //     sex:'nv'
  //   }

  //   var a = JSON.stringify(json);
  //   var b =qs.stringify(json);
  //   console.log('a',a);
  //   console.log('b',b);
  }
}
</script>

<style>

</style>

```

### add.vue

```vue
<template>
    <el-dialog :title="info.title" :visible.sync="info.show">
  <el-form :model="form">
    <el-form-item label="角色名称" :label-width="formLabelWidth">
      <el-input v-model="form.rolename"></el-input>
    </el-form-item>
    <el-form-item label="角色权限" :label-width="formLabelWidth">
        <el-tree
            :data="data"
            show-checkbox
            node-key="id"
            :default-checked-keys="defaultKey"
            :props="defaultProps" ref="tree">
          </el-tree>
    </el-form-item>
     <el-form-item label="角色状态" :label-width="formLabelWidth">
       <el-switch
            v-model="form.status"
            active-color="blue"
            inactive-color="grey" :active-value="1" :inactive-value="2">
          </el-switch>
    </el-form-item>


  </el-form>
  <div slot="footer" class="dialog-footer">
    <el-button @click="cancel">取 消</el-button>
    <el-button type="primary" @click="confirm" v-if="info.isAdd">确 定</el-button>
    <el-button type="primary" @click="update" v-else>修 改</el-button>
  </div>
</el-dialog>
</template>

<script>
import {mapGetters,mapActions} from 'vuex';
import {requestAddRole,requestRoleOne,requestUpdateRole} from '../../../utils/request';
import { successAlert, warningAlert } from '../../../utils/alert';
export default {
  props:['info'],
  computed:{
    ...mapGetters({
      "data":"menu/list"
    })
  },
  data(){
    return {
      form:{
          rolename:'',
          menus:'',
          status:1
         },
      // data: [],
      defaultProps: {
          children: 'children',
          label: 'title'
        },
      defaultKey:[],
      formLabelWidth:'80px'
    }
  },
  methods:{
    ...mapActions({
      "requestMenuList":"menu/listActions",
      "requestRoleList":"role/listActions",
    }),
    // 取消弹框,form置空
    cancel(){
      this.info.show = false;
      this.form = {
          rolename:'',
          menus:'',
          status:1
      },
      // 将树形结构的defaultKey为空
      this.defaultKey = this.$refs.tree.setCheckedKeys([]);
    },
    confirm(){
      this.form.menus = JSON.stringify(this.$refs.tree.getCheckedKeys())
      // 发送添加角色请求
      requestAddRole(this.form).then(res=>{
        if(res.data.code == 200){
          successAlert(res.data.msg)
          this.cancel()
          this.requestRoleList()
        }else{
          warningAlert(res.data.msg)
        }
      })
    },
    // 根据id获取角色的详细信息
    getDetail(id){
      requestRoleOne({id:id}).then(res=>{
        if(res.data.code == 200){
          console.log(JSON.parse(res.data.list.menus));
          this.form = res.data.list;
          this.form.id = id;
          this.defaultKey = JSON.parse(res.data.list.menus)
        }
      })
    },
    update(id){
       this.form.menus = JSON.stringify(this.$refs.tree.getCheckedKeys())
       console.log(this.form);
      requestUpdateRole(this.form).then(res=>{
        if(res.data.code == 200){
          successAlert(res.data.msg)
          this.cancel()
          this.requestRoleList()
        }else{
          warningAlert(res.data.msg)
        }
      })
    }
  },
  mounted(){
    //自定义的函数名
    this.requestMenuList()
  }
}
</script>

<style>

</style>

```

### list.vue

```vue
<template>
  <div>
       <el-table
    :data="list"
    style="width: 100%;margin-bottom: 20px;"
    row-key="id"
    border
    :tree-props="{children: 'children'}">
    <el-table-column  prop="id" label="角色编号"  width="180"> </el-table-column>
    <el-table-column  prop="rolename" label="角色名称"  width="180"> </el-table-column>
    <el-table-column label="状态"  width="180">
       <template slot-scope="scope">
       <el-button type="primary" v-if="scope.row.status == 1">启用</el-button>
       <el-button type="danger" v-else>禁止</el-button>
       </template>
    </el-table-column>
    <el-table-column label="操作"  width="180">
      <template slot-scope="scope">
        <el-button type="primary" @click="edit(scope.row.id)">编辑</el-button>
        <el-button type="danger" @click="del(scope.row.id)">删除</el-button>
      </template>
    </el-table-column>

  </el-table>
  </div>
</template>

<script>
import {mapGetters,mapActions} from  'vuex';
import {requestDelRole} from '../../../utils/request';
import {successAlert} from '../../../utils/alert';
export default {
  computed:{
    ...mapGetters({
      'list':'role/list'
    })
  },
  methods:{
    ...mapActions({
      "requestRoleList":"role/listActions"
    }),
    edit(id){
      // 自定事件传递给父组件
      this.$emit('edit',id);
    },
    del(id){
       this.$confirm('确定要删除吗?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          requestDelRole({id:id}).then(res=>{
            if(res.data.code == 200){
              successAlert(res.data.msg);
               this.requestRoleList()
            }
          })
        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消删除'
          });
        });
    }
  },
  mounted(){
    this.requestRoleList()
  }
}
</script>

<style>

</style>

```

store/module/role.js

```js
import {requestRoleList} from '../../utils/request';
const state = {
  list:[]
}

const mutations = {
  changeList(state,arr){
    state.list = arr
  }
}

const actions = {
  listActions(context){
    // 发起获取角色列表请求
    requestRoleList().then(res=>{
      if(res.data.code = 200){
        context.commit('changeList',res.data.list)
      }
    })
  }
}

const getters ={
  list(state){
    return state.list
  }
}

export default {
  state,
  mutations,
  actions,
  getters,
  namespaced:true
}

最后需要在store/index.js引入
import role from './modules/role';
 modules:{
    menu,
    role,
  }

```

utils/request.js

```
在请求文件中添加有关角色请求
```

## 2.管理员管理

### manager.vue

```vue
<template>
  <div>
    <el-button type="primary" @click="add">添加</el-button>
    <v-add :info="info" ref="add"></v-add>
    <v-list @edit="edit($event)"></v-list>
  </div>
</template>

<script>
import vAdd from './components/add.vue';
import vList from './components/list.vue';
export default {
  data(){
    return {
      info :{
        show:false,
        title:'',
        isAdd:true
      }
    }
  },
  methods:{
    add(){
      this.info.show = true;
      this.info.title = '添加管理员';
      this.info.isAdd = true
    },
    edit(id){
      this.info.show = true;
      this.info.title = '编辑管理员';
      this.info.isAdd = false;
      this.$refs.add.getDetail(id)
    }
  },
  components:{
    vAdd,
    vList
  }
}
</script>

<style>

</style>

```

### add.vue

```vue
<template>
    <el-dialog :title="info.title" :visible.sync="info.show">
  <el-form :model="form">
     <el-form-item label="角色名称" :label-width="formLabelWidth">
      <el-select v-model="form.roleid">
        <el-option label="--请选择--" value="" disabled></el-option>
        <!-- 动态数据获取 -->
        <el-option v-for="item in roleList" :key="item.id" :label="item.rolename" :value="item.id"></el-option>
      </el-select>
    </el-form-item>
    <el-form-item label="用户名" :label-width="formLabelWidth">
      <el-input v-model="form.username"></el-input>
    </el-form-item>
    <el-form-item label="密码" :label-width="formLabelWidth">
      <el-input type="password" v-model="form.password"></el-input>
    </el-form-item>
     <el-form-item label="管理员状态" :label-width="formLabelWidth">
       <el-switch
            v-model="form.status"
            active-color="blue"
            inactive-color="grey" :active-value="1" :inactive-value="2">
          </el-switch>
    </el-form-item>


  </el-form>
  <div slot="footer" class="dialog-footer">
    <el-button @click="cancel">取 消</el-button>
    <el-button type="primary" @click="confirm" v-if="info.isAdd">确 定</el-button>
    <el-button type="primary" @click="update" v-else>修 改</el-button>
  </div>
</el-dialog>
</template>

<script>
import {mapGetters,mapActions} from 'vuex';
import {requestAddManager,requestManagerOne,requestUpdateManager} from '../../../utils/request'
import { successAlert, warningAlert } from '../../../utils/alert';
export default {
  props:['info'],
  computed:{
    ...mapGetters({
      'roleList':'role/list'
    })
  },
  data(){
    return {
       form: {
          roleid:'',
          username:'',
          password:'',
          status:1
        },
        formLabelWidth:'100px'
    }
  },
  methods:{
    ...mapActions({
      "requestRoleList":'role/listActions',
      "requestCount":"manager/countActions",
      "requestManagerList":"manager/listActions"
    }),
    cancel(){
      this.info.show = false;
      this.form = {
          roleid:'',
          username:'',
          password:'',
          status:1
      }
    },
    confirm(){
      // 发起添加管理员请求
      requestAddManager(this.form).then(res=>{
        if(res.data.code == 200){
          successAlert(res.data.msg);
          this.cancel()
          this.requestCount()
          this.requestManagerList()
        }else{
          warningAlert(res.data.msg)
        }
      })
    },
    getDetail(id){
      // 获取管理员详情
      requestManagerOne({uid:id}).then(res=>{
        if(res.data.code == 200){
          this.form = res.data.list;
          this.form.uid = id;
          this.form.password = '';
        }
      })
    },
    update(){
      console.log(this.form);
      requestUpdateManager(this.form).then(res=>{
        if(res.data.code == 200){
          successAlert(res.data.msg);
          this.cancel()
          this.requestCount()
           this.requestManagerList()
        }else{
          warningAlert(res.data.msg)
        }
      })
    }
  },
  mounted(){
    this.requestRoleList()
  }

}
</script>

<style>

</style>

```

### list.vue

```vue
<template>
  <div>
     <el-table
    :data="list"
    style="width: 100%;margin-bottom: 20px;"
    row-key="id"
    border
    :tree-props="{children: 'children'}">
    <el-table-column  prop="id" label="用户编号"  width="180"> </el-table-column>
    <el-table-column  prop="username" label="用户名称"  width="180"> </el-table-column>
    <el-table-column  prop="rolename" label="所属角色"  width="180"> </el-table-column>
    <el-table-column label="状态"  width="180">
       <template slot-scope="scope">
       <el-button type="primary" v-if="scope.row.status == 1">启用</el-button>
       <el-button type="danger" v-else>禁止</el-button>
       </template>
    </el-table-column>
    <el-table-column label="操作"  width="180">
      <template slot-scope="scope">
        <el-button type="primary" @click="edit(scope.row.uid)">编辑</el-button>
        <el-button type="danger" @click="del(scope.row.uid)">删除</el-button>
      </template>
    </el-table-column>
  </el-table>
  <!-- 此处是分页 -->
  <el-pagination  background layout="prev, pager, next" :page-size="size" :total="count" @current-change="cPage">
</el-pagination>
  </div>
</template>

<script>
import {mapGetters,mapActions} from 'vuex';
import {requestDelManager} from '../../../utils/request';
import {successAlert} from '../../../utils/alert';
export default {
  computed:{
    ...mapGetters({
       "count":"manager/count",
      "list":'manager/list',
      // 每页显示的条数
      "size":"manager/size"

    })
  },
  methods:{
    ...mapActions({
      'requestCount':'manager/countActions',
      'requestManagerList':'manager/listActions',
      'requestPage':"manager/pageActions"
    }),
    // 获取当前页码数
    cPage(page){
      this.requestPage(page)
      this.requestManagerList()
    },
    edit(id){
      this.$emit('edit',id)
    },
    del(id){
        this.$confirm('确定要删除吗?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          requestDelManager({uid:id}).then(res=>{
            if(res.data.code == 200){
              successAlert(res.data.msg);
              this.requestCount()
               this.requestManagerList()
            }
          })
        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消删除'
          });
        });
    }
  },
  mounted(){
    this.requestCount()
    this.requestManagerList()
  }
}
</script>

<style>

</style>

```

注意:

```js
在删除操作:主要在store/modules/manager.js中进行删除操作的判断
import {requestMangerList,requestCount} from '../../utils/request';
const state = {
  list:[],
  // 每页显示是的条数
  size:2,
  // 当前页码数
  page:1,
  //总的记录条数
  count:0
}

const mutations = {
  changeList(state,arr){
    state.list = arr
  },
  changeCount(state,n){
    state.count = n
  },
  changePage(state,cPage){
    state.page = cPage
  }
}

const actions = {
  listActions(context){

    // 请求用户列表
    // params = {size:2,page:1}
    var params = {
      size:context.state.size,
      page:context.state.page
    }
    requestMangerList(params).then(res=>{
      if(res.data.code == 200){
        if((res.data.list == null || res.data.list.length==0)&& context.state.page > 1){
          var page = context.state.page - 1;
          context.commit('changePage',page);
          context.dispatch('listActions')
          return
        }
        context.commit('changeList',res.data.list)
      }
    })
  },
  countActions(context){
    // 发起
    requestCount().then(res=>{
      if(res.data.code == 200){
        context.commit('changeCount',res.data.list[0].total)
      }
    })

  },
  pageActions(context,cPage){
    context.commit('changePage',cPage)
  }
}


const getters = {
  list(state){
    return state.list
  },
  count(state){
    return state.count
  },
  size(state){
    return state.size
  }
}

export default {
  state,
  mutations,
  actions,
  getters,
  namespaced:true
}


```

### 3.login.vue(用户信息存储和访问)

```vue
<template>
  <div class="login">
    <div class="con">
      <h3>登录</h3>
      <el-input v-model="user.username" placeholder="请输入用户"></el-input>
      <el-input v-model="user.password" type="password"  placeholder="请输入密码"></el-input>
      <div class="btn">
        <el-button type="primary" @click="login">登录</el-button>
      </div>

    </div>
  </div>
</template>

<script>
import {requestLogin} from '../../utils/request';
import { successAlert, warningAlert } from '../../utils/alert';
import {mapActions} from 'vuex';
 export default {
  data(){
    return {
      user:{
        username:'',
        password:''
      }
    }
  },

  methods:{
    ...mapActions({
      "requestUser":"userActions"
    }),
    login(){
      // 发起登录请求
      //方式二:存储到vuex和sessionStorage
      requestLogin(this.user).then(res=>{
        if(res.data.code == 200){
          successAlert(res.data.msg)
          //将用户信息存入到store,
          this.requestUser(res.data.list)
          this.$router.push('/')
        }else{
          warningAlert(res.data.msg)
        }
      })














      // 方式一
      // requestLogin(this.user).then(res=>{
      //   if(res.data.code == 200){
      //     successAlert(res.data.msg)
      //     // 成了,将用户信息存入localStorage
      //     if(localStorage.getItem('user') !== null){
      //       localStorage.removeItem('user')
      //     }
      //     localStorage.setItem('user',JSON.stringify(res.data.list))
      //     this.$router.push('/')
      //   }else{
      //     warningAlert(res.data.msg)
      //   }
      // })



    }
  }
}
</script>

<style scoped>
.login{
  width:100vw;
  height: 100vh;
  background: linear-gradient(#553443,#2F3D60);
  position: fixed;
  left: 0;
  top:0;
}
.con{
  width:400px;
  height: 300px;
  background: white;
  border-radius: 10px;
  position: absolute;
  left:50%;
  top:50%;
  transform: translate(-50%,-50%);
}
h3{
  text-align: center;
  margin: 20px;
}
.el-input,.el-button{
  width:90%;
  margin:15px;
}
</style>


```

在store/mutaions.js

```vue
export const state = {
  user: sessionStorage.getItem('user') ? JSON.parse(sessionStorage.getItem('user')) : null
}
export const mutations = {
  changeUser(state,arr){
    // 将用户信息存入到store的user中
    state.user = arr
    // 同时将用户存入到sessionStorage中
    if(arr){
      sessionStorage.setItem('user',JSON.stringify(arr))
    }else{
      sessionStorage.removeItem('user')
    }
  }
}
export const getters = {
  user(state){
    return state.user
  }
}


```

在store/actions.js

```js
export default {
    userActions(context,arr){
      context.commit("changeUser",arr)
    }
}


```

### 4.index.vue(菜单遍历)

```vue
<template>
  <el-container>
  <el-aside width="150px">
    <el-menu
      class="el-menu-vertical-demo"
      background-color="#545c64"
      text-color="#fff"
      active-text-color="#ffd04b" :unique-opened="true" router>
       <el-menu-item index="/">
        <i class="el-icon-menu"></i>
        <span slot="title">首页</span>
      </el-menu-item>
      <template v-for="item in user.menus">
        <!-- 这里遍历的是有目录的,即有children -->
           <el-submenu v-if="item.children" :index="item.id" :key="item">
            <template slot="title">
              <i :class="item.icon"></i>
              <span>{{item.title}}</span>
            </template>
            <template v-for="i in item.children">
               <el-menu-item :key="i.id" :index="'/'+i.url">{{i.title}}</el-menu-item>
            </template>
          </el-submenu>
          <!-- 这里显示没有目录的,即没有children -->
          <el-menu-item v-if="!item.children" :key="item.id" :index="'/'+item.url">{{item.title}}</el-menu-item>
      </template>

      <!-- <el-submenu index="2">
        <template slot="title">
          <i class="el-icon-location"></i>
          <span>商城管理</span>
        </template>
          <el-menu-item index="/category">商品分类</el-menu-item>
          <el-menu-item index="/specs">商品规格</el-menu-item>
          <el-menu-item index="/goods">商品管理</el-menu-item>
          <el-menu-item index="/member">会员管理</el-menu-item>
          <el-menu-item index="/banner">轮播管理</el-menu-item>
          <el-menu-item index="/seckill">秒杀活动</el-menu-item>
      </el-submenu> -->
    </el-menu>
  </el-aside>
  <el-container>
    <el-header height="80px">
      <span>admin</span>
      <el-button type="primary">退出</el-button>
    </el-header>
    <el-main>
      <el-breadcrumb separator-class="el-icon-arrow-right">
        <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
        <el-breadcrumb-item>{{$route.name}}</el-breadcrumb-item>
      </el-breadcrumb>
      <router-view></router-view>
      </el-main>
  </el-container>
</el-container>
</template>

<script>
import {mapGetters} from 'vuex'
export default {
  computed:{
    ...mapGetters({
      "user":"user"
    })
  }
}
</script>

<style scoped>
.el-aside{
  height: 100vh;
  background: #333;
  color:white;
}
.el-header{
  /* height: 80px; */
  background: #B3C0D1;
  text-align: right;
  line-height: 80px;
}
.el-breadcrumb{
  margin-bottom: 20px;
}
</style>


```

### 5.request.js

```js
// 请求拦截
// 当用户登录进去之后,需要带着唯一个令牌(token),进行页面的访问
// 在请求拦截中设置token
axios.interceptors.request.use(config=>{
  console.group("本地请求的路径为:",config.url)
  // 从store中取出的user里边的token
  // console.log(store.state.user);
  if(config.url !== baseUrl+'/api/userlogin'){
    config.headers.authorization =store.state.user.token
  }
  return config
})

```

# 一.项目开发

## 1.重定向

```js
  {
      path:'*',
      redirect:'/login',
    }
```



## 2.退出

```vue
<el-header height="80px">
      <span>{{user.username}}</span>
      <el-button type="primary" @click="logout">退出</el-button>
</el-header>
```

```js
 methods:{
    ...mapActions({
      "requestUser":"userActions"
    }),
    logout(){
      // 删除用户信息
      this.requestUser(null)
      this.$router.push('/login');
    }
  }
```



## 3.商品分类

### category.vue

```vue
<template>
  <div>
    <el-button type="primary" @click="add">添加</el-button>
    <v-add :info="info" ref="add"></v-add>
    <v-list @edit="edit($event)"></v-list>
  </div>
</template>

<script>
import vAdd from './componets/add.vue';
import vList from './componets/list.vue';
export default {
  data(){
    return {
      info:{
        show:false,
        title:'',
        isAdd:true
      }
    }
  },
  methods:{
    add(){
      this.info.show = true;
      this.info.title = '添加分类';
      this.info.isAdd = true;
    },
    edit(id){
      this.info.show = true;
      this.info.title = '编辑分类';
      this.info.isAdd = false;
      this.$refs.add.getDetail(id)
    }
  },
  components:{
    vAdd,
    vList
  }
}
</script>

<style>

</style>

```

### add.vue

```vue
<template>
  <div>
    <el-dialog :title="info.title" :visible.sync="info.show">
  <el-form :model="form">
    <el-form-item label="上级分类" :label-width="formLabelWidth">
      <el-select v-model="form.pid">
        <el-option label="--请选择--" value="" disabled></el-option>
        <el-option label="顶级分类" :value="0"></el-option>
        <!-- 动态数据获取 -->
        <el-option v-for="item in list" :key="item.id" :label="item.catename" :value="item.id"></el-option>
      </el-select>
    </el-form-item>
    <el-form-item label="分类名称" :label-width="formLabelWidth">
      <el-input v-model="form.catename"></el-input>
    </el-form-item>
     <el-form-item label="图片" :label-width="formLabelWidth">
       <!-- elementui的文件上传  -->
        <!-- <el-upload
          class="avatar-uploader"
          action="#"
          :show-file-list="false"
           :on-change="changeImg">
          <img v-if="imageUrl" :src="imageUrl" class="avatar">
          <i v-else class="el-icon-plus avatar-uploader-icon"></i>
        </el-upload> -->

        <div class="box-img">
          <h3>+</h3>
          <img v-if="imageUrl" :src="imageUrl" alt="">
          <input type="file" @change="changeImg1">
        </div>
    </el-form-item>
     <el-form-item label="分类状态" :label-width="formLabelWidth">
       <el-switch
            v-model="form.status"
            active-color="blue"
            inactive-color="grey" :active-value="1" :inactive-value="2">
          </el-switch>
    </el-form-item>
  </el-form>
  <div slot="footer" class="dialog-footer">
    <el-button @click="cancel">取 消</el-button>
    <el-button type="primary" @click="confirm" v-if="info.isAdd">确 定</el-button>
    <el-button type="primary" @click="update" v-else>修 改</el-button>
  </div>
</el-dialog>
  </div>
</template>

<script>
import { warningAlert, successAlert } from '../../../utils/alert';
import {requestCateAdd,requestCateOne,requestUpdateCate} from '../../../utils/request';
import {mapGetters,mapActions} from 'vuex';
export default {
  props:['info'],
  computed:{
    ...mapGetters({
      'list':'cate/list'
    })
  },
  data(){
    return {
      imageUrl:'',
      form:{
        pid:0,
        catename:'',
        img:'',
        status:1,
      },
      formLabelWidth:'80px',
    }
  },
  methods:{
    ...mapActions({
      'requestCateList':'cate/listActions'
    }),
    cancel(){
      this.info.show = false;
      this.form = {
        pid:0,
        catename:'',
        img:'',
        status:1,
      },
      this.imageUrl = ''
    },
    changeImg(e){
      console.log(e);
      // 处理文件大小
      if(e.size > 2*1024*1024){
        warningAlert('上传图片不能超过2M');
        return
      }

      // 处理文件后缀
      var ext = ['.jpg','.jpeg','.png','.gif'];
      var extName = e.name.slice(e.name.lastIndexOf('.'))
      if(!ext.some(item=>item==extName)){
        warningAlert('上传文件格式不正确');
        return
      }

      // 生成线上显示的图片
      var file = e.raw;
      // 做生成显示的图片
      this.imageUrl = URL.createObjectURL(file);
      // 将文件信息存入img
      this.form.img = file;
    },
    changeImg1(e){
      var file = e.target.files[0];
      // 处理文件大小
      if(file.size > 2*1024*1024){
        warningAlert('上传图片不能超过2M');
        return
      }

      // 处理文件后缀
      var ext = ['.jpg','.jpeg','.png','.gif'];
      var extName = file.name.slice(file.name.lastIndexOf('.'))
      if(!ext.some(item=>item==extName)){
        warningAlert('上传文件格式有误');
        return
      }

      //显示图片
      this.imageUrl = URL.createObjectURL(file)
      // 将文件信心存入img
      this.form.img = file;
    },
    confirm(){
      requestCateAdd(this.form).then(res=>{
        if(res.data.code == 200){
          successAlert(res.data.msg)
          this.cancel();
          this.requestCateList()
        }else{
          if(res.data.code == 403){
            warningAlert(res.data.msg)
            this.$router.push('/login')
          }else{
             warningAlert(res.data.msg)
          }

        }
      })
    },
    getDetail(id){
      requestCateOne({id:id}).then(res=>{
        if(res.data.code == 200){
          this.form = res.data.list;
          this.form.id = id;
          this.imageUrl = this.$preImg+this.form.img;
        }
      })
    },
    update(){

      requestUpdateCate(this.form).then(res=>{
        if(res.data.code == 200){
          successAlert(res.data.msg)
          this.cancel()
          this.requestCateList()
        }else{
          warningAlert(res.data.msg)
        }
      })
    }
  },
  mounted(){
    this.requestCateList()
  }

}
</script>

<style scoped lang="stylus">
.box-img {
  width 150px;
  height 150px;
  border: 1px dashed pink;
  position relative;
}
.box-img h3{
  width:100%;
  height 100%;
  line-height 150px;
  text-align center;
}
.box-img  img{
  width 100%;
  height 100%;
  position:absolute;
  left:0;
  top:0
}
.box-img input{
   width 100%;
    height 100%;
    position:absolute;
    left:0;
    top:0;
    opacity 0;
}

  .avatar-uploader >>> .el-upload {
    border: 1px dashed #d9d9d9  ;
    border-radius: 6px;
    cursor: pointer;
    position: relative;
    overflow: hidden;
  }
  .avatar-uploader .el-upload:hover {
    border-color: #409EFF;
  }
  .avatar-uploader-icon {
    font-size: 28px;
    color: #8c939d;
    width: 178px;
    height: 178px;
    line-height: 178px;
    text-align: center;
  }
  .avatar {
    width: 178px;
    height: 178px;
    display: block;
  }

</style>

```

自己的图片上传

```
 <div class="box-img">
          <h3>+</h3>
          <img v-if="imageUrl" :src="imageUrl" alt="">
          <input type="file" @change="changeImg1">
        </div>
```

```js
 changeImg1(e){
      var file = e.target.files[0];
      // 处理文件大小
      if(file.size > 2*1024*1024){
        warningAlert('上传图片不能超过2M');
        return
      }

      // 处理文件后缀
      var ext = ['.jpg','.jpeg','.png','.gif'];
      var extName = file.name.slice(file.name.lastIndexOf('.'))
      if(!ext.some(item=>item==extName)){
        warningAlert('上传文件格式有误');
        return
      }

      //显示图片
      this.imageUrl = URL.createObjectURL(file)
      // 将文件信心存入img
      this.form.img = file;
    },
```

```css
.box-img {
  width 150px;
  height 150px;
  border: 1px dashed pink;
  position relative;
}
.box-img h3{
  width:100%;
  height 100%;
  line-height 150px;
  text-align center;
}
.box-img  img{
  width 100%;
  height 100%;
  position:absolute;
  left:0;
  top:0
}
.box-img input{
   width 100%;
    height 100%;
    position:absolute;
    left:0;
    top:0;
    opacity 0;
}
```

### list.vue

```vue
<template>
  <div>
      <el-table
    :data="list"
    style="width: 100%;margin-bottom: 20px;"
    row-key="id"
    border
    :tree-props="{children: 'children'}">
    <el-table-column  prop="id" label="分类编号"  width="180"> </el-table-column>
    <el-table-column  prop="catename" label="分类名称"  width="180"> </el-table-column>
    <el-table-column  prop="img" label="图片"  width="180">
        <template slot-scope="scope">
          <img class="im" :src="$preImg+scope.row.img" alt="">
        </template>
    </el-table-column>
    <el-table-column label="状态"  width="180">
       <template slot-scope="scope">
       <el-button type="primary" v-if="scope.row.status == 1">启用</el-button>
       <el-button type="danger" v-else>禁止</el-button>
       </template>
    </el-table-column>
    <el-table-column label="操作"  width="180">
      <template slot-scope="scope">
        <el-button type="primary" @click="edit(scope.row.id)">编辑</el-button>
        <el-button type="danger" @click="del(scope.row.id)">删除</el-button>
      </template>
    </el-table-column>

  </el-table>
  </div>
</template>

<script>
import {mapGetters,mapActions} from  'vuex';
import {requestDelCate} from '../../../utils/request';
import {successAlert} from '../../../utils/alert';
export default {
  computed:{
    ...mapGetters({
      'list':'cate/list'
    })
  },
  methods:{
    ...mapActions({
      'requestCateList':'cate/listActions',
    }),
    edit(id){
      this.$emit('edit',id)
    },
    del(id){
         this.$confirm('确定要删除吗?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          requestDelCate({id:id}).then(res=>{
            if(res.data.code == 200){
              successAlert(res.data.msg);
               this.requestCateList()
            }
          })
        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消删除'
          });
        });

    }
  },
  mounted(){
    this.requestCateList()
  }
}
</script>

<style scoped>
.im{
  width:100px;
  height: 100px;
}
</style>


```

上传文件是request.js中处理文件格式

```js
// 修改分类
export const requestUpdateCate = (data)=>{
  var form = new FormData()
  for(let i in data){
    form.append(i,data[i])
  }
  return axios({
    method:'post',
    url:baseUrl+'/api/cateedit',
    data:form
  })
}

```



## 4.商品规格

### specs.vue

```vue
<template>
  <div>
    <el-button type="primary" @click="add">添加</el-button>
    <v-add :info="info" ref="add"></v-add>
    <v-list @edit="edit($event)"></v-list>
  </div>
</template>

<script>
import vAdd from './components/add.vue';
import vList from './components/list.vue';
export default {
  data(){
    return {
      info:{
        show:false,
        title:'',
        isAdd:true
      }
    }
  },
  methods:{
    add(){
      this.info.show = true;
      this.info.title = '添加规格';
      this.info.isAdd = true;
    },
    edit(id){
      this.info.show = true;
      this.info.title = '修改规格';
      this.info.isAdd = false;
      this.$refs.add.getDetail(id);
    }
  },
  components:{
    vAdd,
    vList
  }
}
</script>

<style>

</style>


```

### add.vue

```vue
<template>
  <div>
     <el-dialog :title="info.title" :visible.sync="info.show">
  <el-form :model="form">
    <el-form-item label="规格名称" :label-width="formLabelWidth">
      <el-input v-model="form.specsname"></el-input>
    </el-form-item>
    <el-form-item v-for="(item,index) in attrArr" :key="index" label="规格属性" :label-width="formLabelWidth">
      <el-input v-model="item.value"></el-input>
      <el-button v-if="index == 0" type="primary" @click="addAttr">添加规格属性</el-button>
      <el-button v-else type="danger" @click="delAttr(index)">删除</el-button>
    </el-form-item>
     <el-form-item label="规格状态" :label-width="formLabelWidth">
       <el-switch
            v-model="form.status"
            active-color="blue"
            inactive-color="grey" :active-value="1" :inactive-value="2">
          </el-switch>
    </el-form-item>


  </el-form>
  <div slot="footer" class="dialog-footer">
    <el-button @click="cancel">取 消</el-button>
    <el-button type="primary" @click="confirm" v-if="info.isAdd">确 定</el-button>
    <el-button type="primary" @click="update" v-else>修 改</el-button>
  </div>
</el-dialog>
  </div>
</template>

<script>
import { warningAlert, successAlert } from '../../../utils/alert';
import {requestSpecsAdd,requestSpecsOne,requestUpdateSpecs} from '../../../utils/request';
import {mapActions} from 'vuex';
export default {
  props:['info'],
  data(){
    return {
      formLabelWidth:'80px',
      // 放规格属性数组
      attrArr:[{value:''}],
      form:{
        specsname:'',
        attrs:'',
        status:1,
      }
    }
  },
  methods:{
    ...mapActions({
      "requestSpecsList":"specs/listActions",
      "requestSpecsCount":"specs/countActions"
    }),
    addAttr(){
      this.attrArr.push({value:''})
    },
    delAttr(index){
      this.attrArr.splice(index,1);
    },
    cancel(){
      this.info.show = false;
      this.form = {
         specsname:'',
        attrs:'',
        status:1,
      };
      this.attrArr = [{value:''}];
    },
    confirm(){
      // 规格不能为空
      if(this.attrArr.some(item=>item == '')){
        warningAlert('规格属性不能为空');
        return
      }
     this.form.attrs =  this.attrArr.map(item=>item.value).join(',')
      requestSpecsAdd(this.form).then(res=>{
        if(res.data.code == 200){
          successAlert(res.data.msg);
          this.cancel();
          this.requestSpecsCount()
          this.requestSpecsList()
        }else{
          warningAlert(res.data.msg)
        }
      })
    },
    getDetail(id){
      // 获取规格属性详情
      requestSpecsOne({id:id}).then(res=>{
        this.form = res.data.list[0];
        this.form.id = id;
        // 将attrs转换成数组并存到attrArr
        var arr = this.form.attrs.split(',');

        this.attrArr = [];
        for(let i in arr){
          this.attrArr.push({value:arr[i]})
        }
      })
    },
    update(){
       // 规格不能为空
      if(this.attrArr.some(item=>item == '')){
        warningAlert('规格属性不能为空');
        return
      }
      this.form.attrs =  this.attrArr.map(item=>item.value).join(',')
      requestUpdateSpecs(this.form).then(res=>{
        if(res.data.code == 200){
          successAlert(res.data.msg);
          this.cancel()
          this.requestSpecsList()
        }
      })
    }
  }

}
</script>

<style>

</style>


```

### list.vue

```vue
<template>
  <div>
       <el-table
    :data="list"
    style="width: 100%;margin-bottom: 20px;"
    row-key="id"
    border
    :tree-props="{children: 'children'}">
    <el-table-column  prop="id" label="规格编号"  width="180"> </el-table-column>
    <el-table-column  prop="specsname" label="规格名称"  width="180"> </el-table-column>
    <el-table-column  prop="attrs" label="规格属性"  width="180">
      <template slot-scope="scope">
        <el-tag v-for="(item) in scope.row.attrs" :key="item" type="danger">{{item}}</el-tag>
      </template>

    </el-table-column>
    <el-table-column label="状态"  width="180">
       <template slot-scope="scope">
       <el-button type="primary" v-if="scope.row.status == 1">启用</el-button>
       <el-button type="danger" v-else>禁止</el-button>
       </template>
    </el-table-column>
    <el-table-column label="操作"  width="180">
      <template slot-scope="scope">
        <el-button type="primary" @click="edit(scope.row.id)">编辑</el-button>
        <el-button type="danger" @click="del(scope.row.id)">删除</el-button>
      </template>
    </el-table-column>

  </el-table>
   <el-pagination  background layout="prev, pager, next" :page-size="size" :total="count" @current-change="cPage">
</el-pagination>
  </div>
</template>

<script>
import {mapGetters,mapActions} from 'vuex';
import {requestDelSpecs} from '../../../utils/request';
import {successAlert} from '../../../utils/alert';
export default {
  computed:{
    ...mapGetters({
      'list':'specs/list',
      'count':'specs/count',
      'size':'specs/size'
    })
  },
  methods:{
    ...mapActions({
      'requestSpecsCount':'specs/countActions',
      'requestSpecsList':'specs/listActions',
      'requestSpecsPage':'specs/pageActions'
    }),
    // 获取当前页码数
    cPage(page){
      this.requestSpecsPage(page)
      this.requestSpecsList()
    },
    edit(id){
      this.$emit('edit',id)
    },
    del(id){
      this.$confirm('确定要删除吗?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          requestDelSpecs({id:id}).then(res=>{
            if(res.data.code == 200){
              successAlert(res.data.msg);
              this.requestSpecsCount()
               this.requestSpecsList()
            }
          })
        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消删除'
          });
        });
    }
  },
  mounted(){
    this.requestSpecsCount();
    this.requestSpecsList()
  }
}
</script>

<style>

</style>


```



## 5.后台首页

echarts官网:http://echarts.apache.org/zh/index.html

higcharts官网:https://www.highcharts.com.cn/docs/candlestick

安装

```
npm install echarts@4.9.0 --save

```

```vue
<template>
  <div>
    <div id="main"></div>
  </div>
</template>

<script>
import echarts from 'echarts';
export default {
  mounted(){
    var myChart = echarts.init(document.getElementById('main'))
     // 指定图表的配置项和数据
        var option = {
            title: {
                text: '学科'
            },
            tooltip: {},
            legend: {
                data:['就业情况']
            },
            xAxis: {
                data: ["java","大前端","php","python","大数据","UI"]
            },
            yAxis: {},
            series: [{
                name: '就业情况',
                type: 'pie',//饼图
                data: [5000, 20000, 330, 1000, 1000, 200]
            }]
        };
        myChart.setOption(option);
  }
}
</script>

<style scoped>
#main{
  width:600px;
  height: 400px;
  margin:20px auto;
}
</style>


```

# 一.商品管理

## goods.vue

```vue
<template>
  <div>
    <el-button type="primary" @click="add">添加</el-button>
    <v-add :info="info" ref="add"></v-add>
    <v-list @edit="edit($event)"></v-list>
  </div>
</template>

<script>
import vAdd from './components/add.vue';
import vList from './components/list.vue';
export default {
  data(){
    return {
      info:{
        show:false,
        title:'',
        isAdd:true
      }
    }
  },
  methods:{
    add(){
      this.info.show = true;
      this.info.title = '添加商品';
      this.info.isAdd = true
    },
    edit(id){
      this.info.show = true;
      this.info.title = '修改商品';
      this.info.isAdd = false;
      this.$refs.add.getDetail(id)
    }
  },
  components:{
    vAdd,
    vList
  }
}
</script>

<style>

</style>

```

## add.vue

```vue
<template>
  <div>
    <el-dialog :title="info.title" :visible.sync="info.show" @opened="opened">
  <el-form :model="form">
     <el-form-item label="一级分类" :label-width="formLabelWidth">
      <el-select v-model="form.first_cateid" @change="changeCate">
        <el-option label="--请选择--" value="" disabled></el-option>
        <!-- 动态数据获取 -->
        <el-option v-for="item in cateList" :key="item.id" :label="item.catename" :value="item.id"></el-option>
      </el-select>
    </el-form-item>
    <el-form-item label="二级分类" :label-width="formLabelWidth">
      <el-select v-model="form.second_cateid">
        <el-option label="--请选择--" value="" disabled></el-option>
        <!-- 动态数据获取 -->
        <el-option v-for="item in secondCate" :key="item.id" :label="item.catename" :value="item.id"></el-option>
      </el-select>
    </el-form-item>
    <el-form-item label="商品名称" :label-width="formLabelWidth">
      <el-input v-model="form.goodsname"></el-input>
    </el-form-item>
     <el-form-item label="价格" :label-width="formLabelWidth">
      <el-input v-model="form.price"></el-input>
    </el-form-item>
    <el-form-item label="市场价格" :label-width="formLabelWidth">
      <el-input v-model="form.market_price"></el-input>
    </el-form-item>
     <el-form-item label="图片" :label-width="formLabelWidth">
         <div class="box-img">
          <h3>+</h3>
          <img v-if="imageUrl" :src="imageUrl" alt="">
          <input type="file"  @change="changeImg">
        </div>
    </el-form-item>
     <el-form-item label="商品规格" :label-width="formLabelWidth">
      <el-select v-model="form.specsid" @change="changeSpecs">
        <el-option label="--请选择--" value="" disabled></el-option>
        <!-- 动态数据获取 -->
        <el-option v-for="item in specList" :key="item.id" :label="item.specsname" :value="item.id"></el-option>
      </el-select>
    </el-form-item>
     <el-form-item label="规格属性" :label-width="formLabelWidth">
      <el-select v-model="form.specsattr">
        <el-option label="--请选择--" value="" disabled></el-option>
        <!-- 动态数据获取 -->
        <el-option v-for="(item,index) in secondSpecs" :key="index" :label="item" :value="item"></el-option>
      </el-select>
    </el-form-item>
    <el-form-item label="是否新品" :label-width="formLabelWidth">
       <el-switch
            v-model="form.isnew"
            active-color="blue"
            inactive-color="grey" :active-value="1" :inactive-value="2">
          </el-switch>
    </el-form-item>
    <el-form-item label="是否热卖" :label-width="formLabelWidth">
       <el-switch
            v-model="form.ishot"
            active-color="blue"
            inactive-color="grey" :active-value="1" :inactive-value="2">
          </el-switch>
    </el-form-item>
     <el-form-item label="商品状态" :label-width="formLabelWidth">
       <el-switch
            v-model="form.status"
            active-color="blue"
            inactive-color="grey" :active-value="1" :inactive-value="2">
          </el-switch>
    </el-form-item>
     <el-form-item label="商品描述" :label-width="formLabelWidth">
       <div id="editor"></div>
    </el-form-item>
  </el-form>
  <div slot="footer" class="dialog-footer">
    <el-button @click="cancel">取 消</el-button>
    <el-button type="primary" @click="confirm" v-if="info.isAdd">确 定</el-button>
    <el-button type="primary" @click="update" v-else>修 改</el-button>
  </div>
</el-dialog>
  </div>
</template>

<script>
import {mapGetters,mapActions} from 'vuex';
import E from 'wangeditor';
import {requestGoodsAdd,requestGoodssOne,requestUpdateGoods} from  '../../../utils/request';
import { successAlert, warningAlert } from '../../../utils/alert';
export default {
  props:['info'],
  data(){
    return {
      value:null,
      formLabelWidth:'80px',
      // 显示的图片地址
      imageUrl:'',
      // 二级分类列表
      secondCate:'',
      // 规格属性列表
      secondSpecs:'',
      // 富文本的内容
      editor:'',
      form:{
        first_cateid:'',
        second_cateid:'',
        goodsname:'',
        price:'',
        market_price:"",
        img:'',
        description:"",
        specsid:'',
        specsattr:[],
        isnew:1,
        ishot:1,
        status:1
      }
    }
  },
  computed:{
    ...mapGetters({
      "cateList":"cate/list",
      "specList":"specs/list",
    })
  },
  methods:{
    ...mapActions({
      "requestCateList":"cate/listActions",
      "requestSpecsList":"specs/listActions",
      "requestGoodsList":"goods/listActions",
      "requestGoodsCount":"goods/countActions"
    }),
    // 获取二级分类
    changeCate(){
      this.form.second_cateid = '';
      // 先获取一级分类改变时取index
      var index = this.cateList.findIndex(item=>item.id==this.form.first_cateid);
      // 将二级分类放入secondCate
      this.secondCate = this.cateList[index].children;
    },
    // 获取图片信息
    changeImg(e){
      var file = e.target.files[0];
      // 处理文件大小
      if(file.size > 2*1024*1024){
        warningAlert('上传图片不能超过2M');
        return
      }

      // 处理文件后缀
      var ext = ['.jpg','.jpeg','.png','.gif'];
      var extName = file.name.slice(file.name.lastIndexOf('.'))
      if(!ext.some(item=>item==extName)){
        warningAlert('上传文件格式有误');
        return
      }

      //显示图片
      this.imageUrl = URL.createObjectURL(file)
      // 将文件信心存入img
      this.form.img = file;
    },
    // 获取规格属性
    changeSpecs(){
      this.form.specsattr = [];
      var index =this.specList.findIndex(item=>item.id==this.form.specsid);
      // 获取规格属性
      this.secondSpecs = this.specList[index].attrs;
    },
    //dialog打开后的回调
    opened(){
       this.editor = new E('#editor')
        this.editor.create()
        this.editor.txt.html(this.form.description);
    },
    cancel(){
      this.info.show = false;
      this.form = {
         first_cateid:'',
          second_cateid:'',
          goodsname:'',
          price:'',
          market_price:"",
          img:'',
          description:"",
          specsid:'',
          specsattr:[],
          isnew:1,
          ishot:1,
          status:1
      };
      this.imageUrl = '';
    },
    confirm(){
      this.form.description = this.editor.txt.html()
      requestGoodsAdd(this.form).then(res=>{
        if(res.data.code == 200){
          successAlert(res.data.msg);
          this.cancel()
          this.requestGoodsCount()
          this.requestGoodsList(this.value)
        }else{
          warningAlert(res.data.msg)
        }
      })
    },
    getDetail(id){
      //获取商品详情
      requestGoodssOne({id:id}).then(res=>{
        this.form = res.data.list;
        this.form.id = id;
        this.imageUrl = this.$preImg+this.form.img;
      })

    },
    update(){
      this.form.description = this.editor.txt.html()
     requestUpdateGoods(this.form).then(res=>{
        if(res.data.code == 200){
          successAlert(res.data.msg);
          this.cancel()
          this.requestGoodsList(this.value)
        }else{
          warningAlert(res.data.msg)
        }
     })
    }
  },
  mounted(){
    //获取一级分类列表
    this.requestCateList();
    // 获取商品规格
    this.requestSpecsList();

  }
}
</script>

<style scoped lang="stylus">
.box-img {
  width 150px;
  height 150px;
  border: 1px dashed pink;
  position relative;
}
.box-img h3{
  width:100%;
  height 100%;
  line-height 150px;
  text-align center;
}
.box-img  img{
  width 100%;
  height 100%;
  position:absolute;
  left:0;
  top:0
}
.box-img input{
   width 100%;
    height 100%;
    position:absolute;
    left:0;
    top:0;
    opacity 0;
}
</style>

```

## list.vue

```vue
<template>
  <div>
      <el-table
    :data="list"
    style="width: 100%;margin-bottom: 20px;"
    row-key="id"
    border
    :tree-props="{children: 'children'}">
    <el-table-column  prop="id" label="商品编号"  width="180"> </el-table-column>
    <el-table-column  prop="goodsname" label="商品名称"  width="180"> </el-table-column>
    <el-table-column  prop="price" label="价格"  width="180"> </el-table-column>
    <el-table-column  prop="market_price" label="市场价格"  width="180"> </el-table-column>
    <el-table-column  prop="img" label="图片"  width="180">
         <template slot-scope="scope">
          <img class="im" :src="$preImg+scope.row.img" alt="">
        </template>
    </el-table-column>
    <el-table-column  prop="isnew" label="是否新品"  width="180">
       <template slot-scope="scope">
         <el-button type="primary" v-if="scope.row.status == 1">启用</el-button>
       <el-button type="danger" v-else>禁止</el-button>
       </template>
   </el-table-column>
    <el-table-column  prop="ishot" label="是否热卖"  width="180">
       <template slot-scope="scope">
       <el-button type="primary" v-if="scope.row.status == 1">启用</el-button>
       <el-button type="danger" v-else>禁止</el-button>
       </template>
    </el-table-column>
    <el-table-column label="状态"  width="180">
       <template slot-scope="scope">
       <el-button type="primary" v-if="scope.row.status == 1">启用</el-button>
       <el-button type="danger" v-else>禁止</el-button>
       </template>
    </el-table-column>
    <el-table-column label="操作"  width="180">
      <template slot-scope="scope">
        <el-button type="primary" @click="edit(scope.row.id)">编辑</el-button>
        <el-button type="danger" @click="del(scope.row.id)">删除</el-button>
      </template>
    </el-table-column>

  </el-table>
   <el-pagination v-if="value"  background layout="prev, pager, next" :page-size="size" :total="count" @current-change="cPage">
</el-pagination>
  </div>
</template>

<script>
import {mapGetters,mapActions} from 'vuex';
import {requestDelGoods} from '../../../utils/request';
import {successAlert} from '../../../utils/alert';
export default {
  data(){
    return {
      value:null
    }
  },
  computed:{
    ...mapGetters({
      'count':'goods/count',
      'list':"goods/list",
      'size':"goods/size",
    })
  },
  methods:{
    ...mapActions({
      "requestGoodsCount":"goods/countActions",
      "requestGoodsList":"goods/listActions",
      "requestPage":"goods/pageActions",
    }),
    cPage(page){
      this.requestPage(page)
      this.requestGoodsList(this.value)
    },
    edit(id){
      this.$emit('edit',id)
    },
    del(id){
         this.$confirm('确定要删除吗?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          requestDelGoods({id:id}).then(res=>{
            if(res.data.code == 200){
              successAlert(res.data.msg);
              this.requestGoodsCount()
               this.requestGoodsList(this.value)
            }
          })
        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消删除'
          });
        });
    }
  },
  mounted(){
    this.requestGoodsCount()
    this.requestGoodsList(this.value)
  }
}
</script>

<style scoped>
.im{
  width:100px;
  height: 100px;
}
</style>

```

## 富文本编辑器

```js
npm i wangeditor --save
import E from 'wangeditor'
const editor = new E('#main')
editor.create()
```

# 二.项目打包

### 1.在main.js中修改配置

```js
// 开发环境下的配置
// Vue.prototype.$preImg = 'http://localhost:3000';

// 生产环境的配置
Vue.prototype.$preImg = '';
```

### 2.在request.js中修改配置

```js
// 开发环境配置
// const baseUrl = "/api";
// 生产环境的配置
const baseUrl = "";
```

### 3.在生产环境中,将响应拦截和请求拦截中的输出进行关闭

```js
axios.interceptors.request.use(config=>{

  // console.group("本地请求的路径为:",config.url)
  // 从store中取出的user里边的token
  // console.log(store.state.user);
  if(config.url !== baseUrl+'/api/userlogin'){
    config.headers.authorization =store.state.user.token
  }
  return config
})

// 响应拦截
axios.interceptors.response.use(res=>{
  // console.group("本次响应的路径为:"+res.config.url)
  // console.log(res);
  return res
})
```

### 4.打包项目

```
1.npm run build
2.在根目录下生成一个dist目录
3.将dist目录中的所有文件全部放到到后端www
4.通过线上http://localhost:3000进行访问
```

# 三.git

## 1.安装

```
1.全部默认安装
2.在桌面端右键出现git GUI  git bash表示安装成功

```

## 2.使用

本地仓库

```  
1.新建目录:shop
2.打开shop 右键->git bash
3.git init   初始化shop目录,变成本地仓库
4.在shop目录开启的程序之路
5.git add .  将代码添加到暂存区
6.如果是新用户,需要设置全局的用户信息
	git config --global user.email  xxx@xxx.com
	git config --global user.name   xxxx
7.git commit -m '说明'   你修改了什么内容或者你添加什么内容,方便后期查看历史或者版本回退
						将代码提交到本地仓库

```

远程仓库

https://github.com/

```
1.创建一个新的仓库
2.将本地仓库与远程仓库进行关联
	git remote add origin 你的远程仓库地址
3.git push -u origin master  将本地仓库的代码push到远程仓库

```

一个人开发

```
开发一套全新的项目
1.本地新建目录demo
2.将demo目录转换成本地仓库
3.就开启的程序开发和项目管理
4.在github上建立新的远程仓库
5.git  add .
6.git commit -m '说明'
7.git remote add origin 你的远程仓库地址
8.git push -u origin master  
9.git pull 远程仓库地址

```

多人开发

```
1.已经有项目
2.将自己本地的目录变成本地仓库 git init
3.git pull 远程仓库地址
4.开始开启的开发之路
5.git add .  提交本地暂存区
6.git commit -m '说明' 提交本地仓库
7.git remote add origin  远程仓库(只关联一次)
8.每次push代码之前都需要: git pull 远程仓库地址  (为了保证本地的代码与线上的代码版本保持一致)
9.git push -u origin master

```

合并代码:

```
vscode 会帮我自动合并代码
:q!   敲回车即可  表示默认合并

```

代码冲突

```
conflict:冲突
1.当不同的人修改了同一行代码时,会产生冲突.
2.一定需要手动解决
3.git add .
4.git commit -m '说明'
5.git push -u origin matser


```

```
git status  查看当前状态
git log  查看历史版本
git reset --hard 版本号   git reset --hard HEAD~~  回退到上上一个版本

```

# 四.vant

可靠,轻量的移动端的vue组件库

## 1.安装

```
1.创建一个新的vue项目
2.npm i vant -S  
3.npm i babel-plugin-import -D   安装插件
4.修改.babelrc文件
	{
  "presets": [
    ["env", {
      "modules": false,
      "targets": {
        "browsers": ["> 1%", "last 2 versions", "not ie <= 8"]
      }
    }],
    "stage-2"
  ],
  "plugins": ["transform-vue-jsx", "transform-runtime",
    ["import", { "libraryName": "vant", "style": true }]
  ]
}


```

## 2.使用

1.在组件中引入

```
import { Button,Cell, CellGroup,Icon,Popup,Calendar   } from 'vant';

```

2.注册

```
 components:{
    [Button.name]:Button,
    [Cell.name]:Cell,
    [CellGroup.name]:CellGroup,
    [Icon.name]:Icon,
    [Popup.name]:Popup,
    [Calendar.name]:Calendar,
  }

```

3.案例

```vue
<template>
  <div>
    <van-button type="primary">主要按钮</van-button>
    <van-button type="info">信息按钮</van-button>
    <van-cell-group>
      <van-cell title="单元格" value="内容" />
      <van-cell title="单元格" value="内容" label="描述信息" />
    </van-cell-group>
    <van-icon name="chat-o" badge="80" />
    <van-cell is-link @click="showPopup">展示弹出层</van-cell>
    <van-popup v-model="show">内容</van-popup>
    <van-cell title="选择单个日期" :value="date" @click="show = true" />
    <van-calendar v-model="show" @confirm="onConfirm" />
  </div>
</template>

<script>
import { Button,Cell, CellGroup,Icon,Popup,Calendar   } from 'vant';
export default {
   data() {
    return {
      show: false,
       date: '',
    };
  },

  methods: {
    showPopup() {
      this.show = true;
    },
    formatDate(date) {
      return `${date.getMonth() + 1}/${date.getDate()}`;
    },
    onConfirm(date) {
      this.show = false;
      this.date = this.formatDate(date);
    },

  },
  components:{
    [Button.name]:Button,
    [Cell.name]:Cell,
    [CellGroup.name]:CellGroup,
    [Icon.name]:Icon,
    [Popup.name]:Popup,
    [Calendar.name]:Calendar,
  }
}
</script>

<style>

</style>


```

