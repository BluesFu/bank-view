<template>
  <el-table
    :data="tableData"
    style="width: 100%">
    <el-table-column
      label="id"
      width="180">
      <template slot-scope="scope">
        <span style="margin-left: 10px">{{ scope.row.id }}</span>
      </template>
    </el-table-column>

    <el-table-column
      label="username"
      width="180">
      <template slot-scope="scope">
        <span style="margin-left: 10px">{{ scope.row.username }}</span>
      </template>
    </el-table-column>

    <el-table-column
      label="密码"
      width="180">
      <template slot-scope="scope" >
        <span style="margin-left: 10px">{{ scope.row.password }}</span>
      </template>
    </el-table-column>

    <el-table-column
      label="余额"
      width="180">
      <template slot-scope="scope">
        <span style="margin-left: 10px">{{ scope.row.balance }}</span>
      </template>
    </el-table-column>

    <el-table-column
      label="状态"
      width="180">
      <template slot-scope="scope">
        <span style="margin-left: 10px">{{ scope.row.status===false?'封禁':'正常'}}</span>
      </template>
    </el-table-column>

    <el-table-column
      label="身份"
      width="180">
      <template slot-scope="scope">
        <span style="margin-left: 10px">{{ scope.row.admin===false?'普通用户':'管理员'}}</span>
      </template>
    </el-table-column>


    <el-table-column label="操作">
      <template slot-scope="scope">
        <el-button
          size="mini"
          @click="handleUnlock(scope.$index, scope.row)">解封</el-button>
        <el-button
          size="mini"
          type="danger"
          @click="handleBan(scope.$index, scope.row)">封禁</el-button>
      </template>
    </el-table-column>
  </el-table>
</template>


<script>
    export default {
        name: "Admin",
        data() {
            return {
                tableData: [],
            }
        },
        mounted(){
            this.showTableData()
        },

        methods: {
            showTableData(){
                this.$request({
                    method: 'post',
                    url: '/adminPage/admin',
                })
                    .then(res=>{
                        console.log(res);
                        this.tableData=res.data;
                    })
                    .catch(err=>{
                        console.log(err);
                    })
            },
            handleUnlock(index, row) {
                const params = new URLSearchParams();
                params.append('id',row.id);
                this.$request({
                    method: 'post',
                    url: '/adminPage/unlock',
                    data:params
                })
                    .then(res=>{
                      this.showTableData();
                        console.log(res);
                    })
                    .catch(err=>{
                        console.log(err);
                    })
            },
            handleBan(index, row) {
                const params = new URLSearchParams();
                params.append('id',row.id);
                this.$request({
                    method: 'post',
                    url: '/adminPage/ban',
                    data:params
                })
                    .then(res=>{
                        this.showTableData();

                        console.log(res);
                    })
                    .catch(err=>{
                        console.log(err);
                    })
            }
        },

    }
</script>

<style scoped>

</style>
