<template>
  <el-container>
    <el-main>
    <el-form ref="loginForm" :model="form" :rules="rules" label-width="80px" class="login-box">
      <h3 class="login-title">欢迎登录</h3>
      <el-form-item label="账号" prop="username">
        <el-input type="text" placeholder="请输入账号" v-model="form.username"/>
      </el-form-item>
      <el-form-item label="密码" prop="password">
        <el-input type="password" placeh    older="请输入密码" v-model="form.password"/>
      </el-form-item>
      <el-form-item>
        <el-radio v-model="form.role" label="1">管理员</el-radio>
        <el-radio v-model="form.role" label="0">普通用户</el-radio>
      </el-form-item>
      <el-form-item>
        <el-checkbox v-model="remember">记住密码</el-checkbox>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" v-on:click="onSubmit('loginForm')">登录</el-button>
        <el-button type="primary" v-on:click="onCancel('loginForm')">重置</el-button>
      </el-form-item>
      <el-form-item label="还没有账号?" label-width="50%" >
        <el-link  type="primary" @click="register"  >点击注册</el-link>
      </el-form-item>
    </el-form>

    </el-main>
  </el-container>

</template>

<script>

  export default {
    name: "Login",
    data() {
      return {

        form: {
          username:'',
          password:'',
          role: '0'
        },
          remember:false,
        // 表单验证，需要在 el-form-item 元素中增加 prop 属性
        rules: {
          username: [
            {required: true, message: '账号不可为空', trigger: 'blur'}
          ],
          password: [
            {required: true, message: '密码不可为空', trigger: 'blur'}
          ]
        },
      }
    },
    methods: {


      onSubmit(formName) {
        // 为表单绑定验证功能
        this.$refs[formName].validate((valid) => {
          if (valid) {
            var params = new URLSearchParams();
            params.append('username', this.form.username);
            params.append('password', this.form.password);
            params.append('role',this.form.role);

            // 使用 vue-router 路由到指定页面，该方式称之为编程式导航
           this.$request({
               method: 'post',
               url: '/doLogin',
               data: params,

           })
              .then(res=>{
                if(res.data['result']==='success'){
                    this.suc();
                    sessionStorage.setItem('token',res.data['token']);
                    sessionStorage.setItem('name',this.form.username);
                    if (this.form.role==='1'){
                        this.$router.push({
                            name: 'adminPage',
                        });
                    }else {
                        this.$router.push({
                            name: 'function',
                        });
                    }

                }else{
                    this.err('账号或者密码错误,请重新输入！');
                    oncancel(formName);
                }
              })
              .catch(err=>{
                  this.err('网络错误，服务器连接失败！');
               // console.log(err);
              })

          } else {

            return false;
          }
        });
      },
      onCancel(formName) {
        this.$refs[formName].resetFields();
      },
      register(){
        this.$router.push('/register')
      },
        err(msg) {
            this.$message({
                showClose: true,
                message: msg,
                type: 'error'
            });
        },
        suc() {
            this.$message({
                showClose: true,
                message: '登录成功',
                type: 'success'
            });
        },
    }
  }
</script>

<style lang="scss" scoped>
  .login-box {
    border: 1px solid #DCDFE6;
    width: 350px;
    margin: 180px auto;
    padding: 35px 35px 15px 35px;
    border-radius: 5px;
    -webkit-border-radius: 5px;
    -moz-border-radius: 5px;
    box-shadow: 0 0 25px #909399;
  }

  .login-title {
    text-align: center;
    margin: 0 auto 40px auto;
    color: #303133;
  }
</style>
