<template>
  <el-container class="home-container">
    <!-- 头部区域 -->
  <el-header> 
    <div>
      <img src="../assets/guan.png" alt="" class="logo">
      <span>魔励后台管理系统</span>
    </div>
    <el-button type="info" @click="logout">退出</el-button>
  </el-header>
  <!-- 页面主体区域 -->
  <el-container>
    <!-- 侧边栏 -->
    <el-aside :width="isCollapse ? '64px' :'200px'">
      <div class="taggle-button" @click="toggleCollapse">|||</div>
       <el-menu background-color="#5e7c85" text-color="#fff" :unique-opened="true" :collapse="isCollapse" :collapse-transition="false"  router :default-active="activePath" active-text-color="#76becc">
         <!-- 一级菜单 -->
      <el-submenu :index="item.id + ''" v-for="item in menulist" :key="item.id">
        <!-- 一级菜单的模版区域 -->
        <template slot="title">
          <!-- 图标 -->
          <i :class="iconsObj[item.id]"></i>
          <!-- 文本 -->
          <span>{{item.authName}}</span>
        </template>
        <!-- 二级菜单 -->
          <el-menu-item  :index="'/' + subItem.path" v-for="subItem in item.children" :key="subItem.id" @click="saveNavState('/' + subItem.path)">
             <template slot="title">
          <!-- 图标 -->
          <i class="el-icon-menu"></i>
          <!-- 文本 -->
          <span>{{subItem.authName}}</span>
        </template>
          </el-menu-item>
       
      </el-submenu>
     
    </el-menu>
    </el-aside>
    <!-- 右侧内容主体 -->
    <el-main>
<!-- 路由占位符 -->
<router-view></router-view>
    </el-main>
  </el-container>
</el-container>
</template>

<script>
export default {
  data() {
return {
  menulist:[],
  iconsObj:{
    '125':'iconfont icon-user',
    '103':'iconfont icon-tijikongjian',
    '101':'iconfont icon-shangpin',
    '102':'iconfont icon-danju',
    '145':'iconfont icon-baobiao'
  },
   // 定义一个布尔值
  isCollapse:false,
// 被激活的链接地址
   activePath:''
}
  },
  // 定义生命周期函数
  created(){
this.getMenuList()
this.activePath=window.sessionStorage.getItem('activePath')

  },
  methods: {
    logout() {
      //情况token
      window.sessionStorage.clear()
      //跳转到登录页
      this.$router.push('/login')
    },
    // 获取所有的菜单
    async getMenuList(){
      const {data: res} = await this.$http.get('menus')
      if(res.meta.status !==200) return this.$message.error(res.meta.msg)
      this.menulist=res.data
      console.log(res);
      
    },
  // 点击按钮切换菜单的折叠与展开
    toggleCollapse(){
      this.isCollapse=!this.isCollapse
    },
    // 保存链接的激活状态
    saveNavState(activePath){
      window.sessionStorage.setItem('activePath',activePath)
      this.activePath=activePath
    }
  }
}
</script>

<style lang="less" scoped>
.home-container{
  height:100%;
}
.el-header{
  background-color:#26B9CD;
  display: flex;
  justify-content: space-between;
  padding-left:0;
  align-items: center;
  color:#fff;
  font-size:20px;
  > div {
    display: flex;
    align-items: center;
    span{
      margin-left:15px;
    }
  }
}
.logo{
  height: 77%;
  width: 16%;
  background-size:100%100%;
  padding-top: 1px;

}
.el-aside{
  background-color:#5e7c85;
  el-menu{
    border-right: none;
  }
}
.el-main{
  background-color: #d3d7d4;
}
.iconfont{
  margin-right:10px;
}
.taggle-button{
  background-color: #4A5064;
  font-size: 10px;
  line-height: 24px;
  color:#fff;
  text-align: center;
  letter-spacing: 0.2em;
  cursor: pointer;
}
</style>