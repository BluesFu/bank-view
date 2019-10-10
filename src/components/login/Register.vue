<template>
  <el-container>

    <el-main>
      <el-form ref="registerForm" :model="form" :rules="rules" label-width="80px" class="register-box">
        <h3 class="register-title">注册页面</h3>
        <el-form-item label="账号" prop="username">
          <el-input type="text" placeholder="请输入账号" v-model="form.username"/>
        </el-form-item>
        <el-form-item label="密码" prop="password">
          <el-input type="password" placeholder="请输入密码" v-model="form.password"/>
        </el-form-item>
        <el-form-item label="密码" prop="rePassword">
          <el-input type="password" placeholder="请再次输入密码" v-model="form.rePassword"/>
        </el-form-item>

        <el-form-item>
          <el-button type="primary" v-on:click="onSubmit('registerForm')">注册</el-button>
          <el-button type="primary" v-on:click="onCancel('registerForm')">重置</el-button>
        </el-form-item>
        <el-form-item label="已经有账号了?" label-width="50%" >
          <el-link  type="primary" @click="login" >返回登录</el-link>
        </el-form-item>
      </el-form>

    </el-main>
  </el-container>

</template>

<script>

  export default {
    name: "Register",
    data() {
      var checkSame = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('请再次输入密码'));
        } else if (value !== this.form.password) {
          callback(new Error('两次输入密码不一致!'));
        } else {
          callback();
        }
      };
      return {

        form: {
          username: '',
          password: '',
          rePassword:''
        },

        // 表单验证，需要在 el-form-item 元素中增加 prop 属性
        rules: {
          username: [
            {required: true, message: '账号不可为空', trigger: 'blur'}
          ],
          password: [
            {required: true, message: '密码不可为空', trigger: 'blur'}
          ],
          rePassword: [
            {required: true, message: '密码不可为空', trigger: 'blur'},
            { validator: checkSame,trigger: 'blur' }
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
            // 使用 vue-router 路由到指定页面，该方式称之为编程式导航
            this.$request.post('/doRegister',params)
              .then(
                res=> {
                  if (res.data['result']==='success'){
                    this.suc();
                    this.$router.push('/login');
                  }
                }
              ).catch(  err=>{
                this.err('服务器连接失败！')
              }

            )

          } else {
            return false;
          }
        });
      },
      onCancel(formName) {
        this.$refs[formName].resetFields();
      },
      login(){
        this.$router.push('/login')
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
                message: '注册成功,自动跳转到登录页面',
                type: 'success'
            });
        },
    }
  }
</script>

<style lang="scss" scoped>
  .register-box {
    border: 1px solid #DCDFE6;
    width: 350px;
    margin: 180px auto;
    padding: 35px 35px 15px 35px;
    border-radius: 5px;
    -webkit-border-radius: 5px;
    -moz-border-radius: 5px;
    box-shadow: 0 0 25px #909399;
  }

  .register-title {
    text-align: center;
    margin: 0 auto 40px auto;
    color: #303133;
  }
</style>
