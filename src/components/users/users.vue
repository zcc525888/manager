<!--  -->
<template>
  <el-card class="box-card">
    <el-breadcrumb separator="/">
      <el-breadcrumb-item>首页</el-breadcrumb-item>
      <el-breadcrumb-item>用户管理</el-breadcrumb-item>
      <el-breadcrumb-item>用户列表</el-breadcrumb-item>
    </el-breadcrumb>

    <el-row class="searchRow">
      <el-col>
        <el-input
          @clear="loadUserList"
          clearable
          placeholder="请输入内容"
          v-model="query"
          class="inputsearch"
        >
          <el-button
            @click="searchUser"
            slot="append"
            icon="el-icon-search"
          ></el-button>
        </el-input>

        <el-button type="success" @click="showAddUserDia">添加用户</el-button>
      </el-col>
    </el-row>

    <el-table :data="userlist" style="width: 100%">
      <el-table-column type="index" label="#" width="60">
        
       <template slot-scope="scope">
        <span>{{(pagenum-1)*pagesize+scope.$index + 1}}</span>
      </template>
      
      </el-table-column>
      <el-table-column prop="username" label="姓名" width="80">
      </el-table-column>
      <el-table-column prop="email" label="邮箱"> </el-table-column>
      <el-table-column prop="mobile" label="电话"> </el-table-column>
      <el-table-column prop="create_time" label="创建日期"> </el-table-column>

      <!-- <el-table-column prop="create_time" label="创建日期"> 
        <template slot-scope="userlist">
          {{userlist.row.create_time|fmtdate}}
        </template>
      </el-table-column> -->

      <el-table-column label="用户状态">
        <template slot-scope="userlist">
          <el-switch
            :value="Boolean(userlist.row.is_active)"
            active-color="#13ce66"
            inactive-color="#ff4949"
            @change="changeState(userlist.row)"
          >
          </el-switch>
        </template>
      </el-table-column>

      <el-table-column prop="address" label="操作">

        <template slot-scope="scope">
          <el-button
            size="mini"
            plain
            type="primary"
            icon="el-icon-edit"
            circle
            @click="showEditUserDia(scope.row)"
          ></el-button>
     

          <el-button
            size="mini"
            plain
            type="danger"
            icon="el-icon-delete"
            circle
            @click="showDeleteUseMsgBox(scope.row.id)"
          ></el-button>
         

          <el-button
            size="mini"
            plain
            type="success"
            icon="el-icon-check"
            circle
            @click="showUserRoleDia(scope.row)"
          ></el-button>
        </template>
        
      </el-table-column>
    </el-table>

    <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="pagenum"
      :page-sizes="[2, 4, 6, 8]"
      :page-size="pagesize"
      layout="total, sizes, prev, pager, next, jumper"
      :total="total"
    >
    </el-pagination>

    <el-dialog title="添加用户" :visible.sync="dialogFormVisible">
      <el-form :model="form">
        <el-form-item label="用户名" label-width="100px">
          <el-input v-model="form.username" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="密码" label-width="100px">
          <el-input v-model="form.password" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="邮箱" label-width="100px">
          <el-input v-model="form.email" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="电话" label-width="100px">
          <el-input v-model="form.mobile" autocomplete="off"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="addUser"
          >确 定</el-button
        >
      </div>
    </el-dialog>



    <el-dialog title="添加用户" :visible.sync="dialogFormEdit">
      <el-form :model="form">
        <el-form-item label="用户名" label-width="100px">
          <el-input disabled v-model="form.username" autocomplete="off"></el-input>
        </el-form-item>
 
        <el-form-item label="邮箱" label-width="100px">
          <el-input v-model="form.email" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="电话" label-width="100px">
          <el-input v-model="form.mobile" autocomplete="off"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormEdit = false">取 消</el-button>
        <el-button type="primary" @click="editUser"
          >确 定</el-button
        >
      </div>
    </el-dialog>

    <el-dialog title="分配角色" :visible.sync="dialogFormRole">
  <el-form :model="form">
    <el-form-item label="用户名" label-width="100px">
      当前用户的用户名
    </el-form-item>
    <el-form-item label="角色" label-width="100px">

      {{curRoleId}}
      <el-select v-model="curRoleId" >
        <el-option label="请选择" :value="-1"></el-option>
        <el-option :label="item.role_name" :value="item.role_id" v-for="(item,i) in roles" :key="i"></el-option>
      </el-select>
    </el-form-item>
  </el-form>
  <div slot="footer" class="dialog-footer">
    <el-button @click="dialogFormRole = false">取 消</el-button>
    <el-button type="primary" @click="setRole">确 定</el-button>
  </div>
</el-dialog>




  </el-card>
</template>

<script>
// import { use } from 'vue/types/umd';
//这里可以导入其他文件（比如：组件，工具js，第三方插件js，json文件，图片文件等等）
//例如：import 《组件名称》 from '《组件路径》';

export default {
  //import引入的组件需要注入到对象中才能使用
  components: {},
  data() {
    //这里存放数据
    return {
      query: "",

      userlist: [],
      total: -1,
      pagenum: 1,
      pagesize: 2,

      dialogFormVisible:false,
      dialogFormEdit:false,
      dialogFormRole:false,
      curRoleId:-1,
      curUserName:"",
      curUserId:-1,
      roles:[],
      form:{
        username:'',
        password:'',
        email:'',
        mobile:'',
      }
    };
  },
  //监听属性 类似于data概念
  computed: {
    getIndex(){

    }
  },
  //监控data中的数据变化
  watch: {},
  //方法集合
  methods: {
    setRole(){
         this.$http.post(`role/setrole`,{
           user_id:this.curUserId,
           role_id:this.curRoleId
         }).then(
        (res)=>{
          console.log(res)
          if(res.data.status===200){
             this.$message.success(res.data.msg)
          }else{
             this.$message.warning(res.data.msg)
          }
          this.dialogFormRole=false
        }
      )
    },
    showUserRoleDia(user){
      console.log(user)
      this.curUserName=user.username
      this.curUserId=user.id
      this.dialogFormRole=true

      this.$http.get(`role/roles`).then(
           (res)=>{
             console.log(res.data);
             this.roles=res.data.data;
           }
         )
      this.$http.get(`user/userwithRole/${user.id}`).then(
        (res)=>{
          if(res.data.status===200){
          this.curRoleId=res.data.data.role_id;
          }else{
            this.curRoleId=-1;
          }
        }
      )
    },
    changeState(user){
      console.log(user.id)
      console.log(user.is_active)
         this.$http.put(`user/edituser/${user.id}/status/${user.is_active}`).then(
           (res)=>{
             this.dialogFormEdit=false
             this.getUserList()
           }
         )
    },
    editUser(){
      //这个接口还没写


         this.$http.put(`user/edituser/${this.form.id}`,this.form).then(
           (res)=>{
             this.dialogFormEdit=false
             this.getUserList()
           }
         )
    },
    showEditUserDia(user){
      this.dialogFormEdit=true
      this.form=user

    },
    showDeleteUseMsgBox(userId){

      console.log(userId)
      this.$confirm('确定删除用户吗?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$http.delete(`user/deleteuser/${userId}`).then(
            (res)=>{
              if(res.status===200){
                this.pagenum=1
                this.getUserList()
                   this.$message({
                    type: 'success',
                    message: res.data.msg
                  });
              }

            }
          )
       
        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消删除'
          });          
        });
    },
    
    addUser(){
      this.dialogFormVisible=false
      this.$http.post(`user/adduser`,this.form).then(
        (res)=>{
            const{data,msg,status}=res.data
            if(status===201){
              this.$message.success(msg)
              this.getUserList()
              this.form={}
            }else{
              this.$message.warning(msg)
            }
        }
      )
    },
    showAddUserDia(){
      this.form={}
      this.dialogFormVisible=true;
    },
    searchUser() {
      console.log("ssss");
      this.getUserList();
    },
    loadUserList() {
      this.getUserList();
    },
    handleSizeChange(val) {
      this.pagesize = val;
      this.getUserList();
    },
    handleCurrentChange(val) {
      this.pagenum = val;
      this.getUserList();
    },
    getUserList() {
      const AUTH_TOKEN = localStorage.getItem("token");
      this.$http.defaults.headers.common["Authorization"] = AUTH_TOKEN;
      this.$http
        .get(
          `user/users?query=${this.query}&pagenum=${this.pagenum}&pagesize=${this.pagesize}`
        )
        .then((res) => {
          console.log(res);

          const { data, total, status, msg } = res.data;
          if (status === 200) {
            this.userlist = data;
            this.total = total;
            this.$message.success(msg);
          } else {
            this.$message.warning(msg);
          }
        });
      // this.userlist = [{
      //   id: 2,
      //   rid: null,
      //   username: "aa",
      //   password: null,
      //   mobile: "11",
      //   email: "22",
      //   create_time: "2020-12-04",
      //   modify_time: null,
      //   is_delete: 1,
      //   is_active: true,
      // }];
    },
  },
  //生命周期 - 创建完成（可以访问当前this实例）
  created() {
    
    this.getUserList();
  },
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
<style scoped>
.box-card {
  height: 100%;
}
.inputsearch {
  width: 300px;
}
.searchRow {
  margin-top: 20px;
}
</style>