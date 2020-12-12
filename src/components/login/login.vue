<!--  -->
<template>
  <div class="login-wrap">
    <el-form
      class="login-form"
      label-position="top"
      label-width="80px"
      :model="formdata"
    >
        <h2>用户登录</h2>
      <el-form-item label="用户名">
        <el-input v-model="formdata.username"></el-input>
      </el-form-item>
      <el-form-item label="密码">
        <el-input v-model="formdata.password"></el-input>
      </el-form-item>
      <el-button type="primary" class="login-btn" @click.prevent="handleLogin">登录</el-button>
   
    </el-form>
  </div>
</template>

<script>
//这里可以导入其他文件（比如：组件，工具js，第三方插件js，json文件，图片文件等等）
//例如：import 《组件名称》 from '《组件路径》';

export default {
  //import引入的组件需要注入到对象中才能使用
  components: {},
  data() {
    //这里存放数据
    return {
        formdata:{
            username:'',
            password:'',
        }
    };
  },
  //监听属性 类似于data概念
  computed: {},
  //监控data中的数据变化
  watch: {},
  //方法集合
  methods: {
    handleLogin(){
      this.$http.post('user/login',this.formdata).then((res)=>{
        const {data,msg,status}=res.data
        console.log(status)
        if(status===200){

          localStorage.setItem('token',data.token);
          // this.$router.push({path:'/'})
           this.$router.push({name:'home'})
          this.$message.success("成功哦，"+msg);
        }else{
          this.$message.warning("警告哦，"+msg);
        }
      })

    }
  },
  //生命周期 - 创建完成（可以访问当前this实例）
  created() {},
  //生命周期 - 挂载完成（可以访问DOM元素）
  mounted() {},
  beforeCreate() {}, //生命周期 - 创建之前
  beforeMount() {}, //生命周期 - 挂载之前
  beforeUpdate() {}, //生命周期 - 更新之前
  updated() {}, //生命周期 - 更新之后
  beforeDestroy() {}, //生命周期 - 销毁之前
  destroyed() {}, //生命周期 - 销毁完成
  activated() {}, //如果页面有keep-alive缓存功能，这个函数会触发
};
</script>
<style>
.login-wrap{
    height: 100%;
    background-color: #324152;
    
    display: flex;
    justify-content: center;
    align-items: center;
}
 .login-form{
    width:400px ;
    background-color: #fff;
    border-radius: 5px;
    padding: 30px;
}
.login-btn{
    width: 100%;
}
</style>