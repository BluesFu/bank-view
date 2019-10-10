<template>
  <el-form :model="numberValidateForm" ref="numberValidateForm" label-width="100px" class="demo-ruleForm">
    <el-form-item
      label="转账金额"
      prop="money"
      :rules="[
      { required: 'true', message: '金额不能为空'},
      { type: 'string', message: '金额必须为数字值'}
    ]"
    >
      <el-input type="money"  v-model="numberValidateForm.money"  autocomplete="off"></el-input>
    </el-form-item>
    <el-form-item
    label="转入账户"
    prop="name"
    :rules="[
    {required: 'true', message: '名字不能为空'}
    ]"
    >
    <el-input type="name" v-model="numberValidateForm.name" autocomplete="off"></el-input>

    </el-form-item>
    <el-form-item>
      <el-button type="primary" @click="submitForm('numberValidateForm')">提交</el-button>
      <el-button @click="resetForm('numberValidateForm')">重置</el-button>
    </el-form-item>
  </el-form>
</template>

<script>
  export default {
    name: "Transfer",
    data() {
      return {
        numberValidateForm: {
          money: '',
          name: ''
        }
      };
    },
    methods: {
      submitForm(formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {
            if(this.numberValidateForm.money.match('^[+]{0,1}(\\d+)$|^[+]{0,1}(\\d+\\.\\d+)$')){

                var params = new URLSearchParams();
                params.append('money', this.numberValidateForm.money);
                params.append('name',this.numberValidateForm.name);
              this.$request({
                  method: 'post',
                  url: 'userpage/doTransfer',
                  data:params
              })
                  .then(res=>{
                      if (res.data['result']==='success') {
                          this.suc()
                      }else {
                          this.err('余额不足，转账失败')
                      }
                  })
                  .catch(err=>{
                      console.log(err);
                      this.err('服务器连接失败,请稍后再试！')
                  })


            }else {
              this.$message.error('金额输入格式有误,请重新输入!');
              this.$refs[formName].resetFields();
            }
          } else {
            console.log('error submit!!');
            return false;
          }
        });
      },
      resetForm(formName) {
        this.$refs[formName].resetFields();
      },
        suc() {
            this.$notify({
                title: '成功',
                message: '取款成功',
                type: 'success'
            })
        },
        err(msg) {
            this.$notify.error({
                title: '错误',
                message: msg
            })
        }
    }
  }
</script>

<style scoped>

</style>
