<template>
  <el-table
    :data="logs"
    border
    style="width: 100%">
    <el-table-column
      prop="date"
      label="日期"
      width="180">
    </el-table-column>
    <el-table-column
      prop="type"
      label="操作类型"
      width="180">
    </el-table-column>
    <el-table-column
      prop="detail"
      label="操作明细">
    </el-table-column>
  </el-table>

</template>

<script>
    export default {
        name: "Record",

      data() {
        return {

          logs:[]
            }

      },
      mounted() {
            this.$request.post('userpage/doLog')
                .then(res=>{
                    console.log(res);
                    this.logs=res.data;
                })
                .catch(err=>{
                })
        }

    }
</script>

<style scoped>

</style>
