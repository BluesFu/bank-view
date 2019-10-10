<template>
  <el-container>
    <el-header>
      <label >存款页面</label>
    </el-header>
    <el-main>
  <el-form :model="numberValidateForm" ref="numberValidateForm" label-width="100px" class="demo-ruleForm">
    <el-form-item
      label="存款金额"
      prop="money"
      :rules="[
      { required: 'true', message: '金额不能为空'},
      { type: 'string', message: '金额必须为数字值'}
    ]"
    >
      <el-input type="money"  v-model="numberValidateForm.money"  autocomplete="off"></el-input>
    </el-form-item>
    <el-form-item>
      <el-button type="primary" @click="submitForm('numberValidateForm')">提交</el-button>
      <el-button @click="resetForm('numberValidateForm')">重置</el-button>
    </el-form-item>
  </el-form>
    </el-main>
  </el-container>
</template>

<script>
    export default {
        name: "Deposit",
        data() {
            return {
                numberValidateForm: {
                    money: ''
                }
            };
        },
        methods: {
            submitForm(formName) {
                this.$refs[formName].validate((valid) => {
                    if (valid) {
                        if (this.numberValidateForm.money.match('^[+]{0,1}(\\d+)$|^[+]{0,1}(\\d+\\.\\d+)$')) {
                            //  alert('ok')
                            var params = new URLSearchParams();
                            params.append('money', this.numberValidateForm.money);
                            // 使用 vue-router 路由到指定页面，该方式称之为编程式导航
                            this.$request({
                                method: 'post',
                                url: 'userpage/doDeposit',
                                data: params,

                            }).then(res => {
                                this.suc()
                            })
                                .catch(err => {
                                      this.err()
                                })

                        } else {
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
                    message: '存款成功',
                    type: 'success'
                })
            },
            err() {
                this.$notify.error({
                    title: '错误',
                    message: '存款失败'
                })
            }
        }
    }
</script>

<style scoped>

</style>
