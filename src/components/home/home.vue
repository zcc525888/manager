<!--  -->
<template>
  <el-container class="container">
    <el-header class="header">
      <el-row>
        <el-col :span="4"
          ><div class="grid-content bg-purple"><p>哈哈哈</p></div></el-col
        >
        <el-col :span="18" class="middle">
          <h3>管理后台系统</h3>
        </el-col>
        <el-col :span="2">
          <div class="grid-content bg-purple">
            <a href="#" @click.prevent="handleSignout()" class="loginout">退出</a>
          </div>
        </el-col>
      </el-row>
    </el-header>

    <el-container>
      <el-aside width="200px" class="aside">

        <el-menu :router="true">
          <el-submenu index="1">
            <template slot="title">
              <i class="el-icon-location"></i>
              <span>用户管理</span>
            </template>
              <el-menu-item index="users">
                  <i class="el-icon-menu"></i>
              <span>用户列表</span>
              </el-menu-item>
          </el-submenu>

               <el-submenu index="2">
            <template slot="title">
              <i class="el-icon-location"></i>
              <span>权限管理</span>
            </template>
              <el-menu-item index="2-1">
                  <i class="el-icon-menu"></i>
              <span>角色列表</span>
              </el-menu-item>

              <el-menu-item index="2-1">
                  <i class="el-icon-menu"></i>
              <span>权限列表</span>
              </el-menu-item>
          </el-submenu>

               <el-submenu index="3">
            <template slot="title">
              <i class="el-icon-location"></i>
              <span>商品管理</span>
            </template>
              <el-menu-item index="2-1">
                  <i class="el-icon-menu"></i>
              <span>商品列表</span>
              </el-menu-item>
              <el-menu-item index="2-2">
                  <i class="el-icon-menu"></i>
              <span>分类参数</span>
              </el-menu-item>
              <el-menu-item index="2-3">
                  <i class="el-icon-menu"></i>
              <span>商品分类</span>
              </el-menu-item>
          </el-submenu>

               <el-submenu index="4">
            <template slot="title">
              <i class="el-icon-location"></i>
              <span>订单管理</span>
            </template>
              <el-menu-item index="1-1">
                  <i class="el-icon-menu"></i>
              <span>订单列表</span>
              </el-menu-item>
          </el-submenu>

               <el-submenu index="5">
            <template slot="title">
              <i class="el-icon-location"></i>
              <span>数据统计</span>
            </template>
              <el-menu-item index="1-1">
                  <i class="el-icon-menu"></i>
              <span>数据报表</span>
              </el-menu-item>
          </el-submenu>
          
        </el-menu>
      </el-aside>
      <el-main class="main">
          <router-view></router-view>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
//这里可以导入其他文件（比如：组件，工具js，第三方插件js，json文件，图片文件等等）
//例如：import 《组件名称》 from '《组件路径》';

export default {
  //import引入的组件需要注入到对象中才能使用
  components: {},
  data() {
    //这里存放数据
    return {};
  },
  //监听属性 类似于data概念
  computed: {},
  //监控data中的数据变化
  watch: {},
  //方法集合
  methods: {
      handleSignout(){
          localStorage.clear()
          this.$message.success('退出成功')
          this.$router.push({name:'login'})
      }
  },
  //生命周期 - 创建完成（可以访问当前this实例）
  created() {},
  //生命周期 - 挂载完成（可以访问DOM元素）
  mounted() {},
  beforeCreate() {
      const token=localStorage.getItem('token')
      
      if(!token){
          this.$router.push({name:'login'})
      }
  }, //生命周期 - 创建之前
  beforeMount() {}, //生命周期 - 挂载之前
  beforeUpdate() {}, //生命周期 - 更新之前
  updated() {}, //生命周期 - 更新之后
  beforeDestroy() {}, //生命周期 - 销毁之前
  destroyed() {}, //生命周期 - 销毁完成
  activated() {}, //如果页面有keep-alive缓存功能，这个函数会触发
};
</script>
<style scoped>
.container {
  height: 100%;
}
.header {
  background-color: #b3c0d1;
}
.aside {
  background-color: #d3dce6;
}
.main {
  background-color: #e9eef3;
}

.middle {
  text-align: center;
}
.loginout {
  line-height: 70px;
  text-decoration: none;
}
</style>