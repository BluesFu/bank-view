<template>
  <div>
 <label>当前余额：{{balance}}元</label>

  </div>
</template>

<script>
    export default {
        name: "Inquire",

        mounted(){
            this.$request({
                method: 'post',
                url: 'userpage/doInquire',
            }).then(res => {
                this.balance=res.data['balance'];
            })
                .catch(err => {
                    this.balance='0';
                    console.log(err);
                });
        } ,
        data(){
            return{
                balance:''
            }
        },


    }
</script>

<style scoped>

</style>
