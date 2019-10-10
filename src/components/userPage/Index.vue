<template>
  <el-container style="height: 150vh; border: 1px solid #eee">
    <el-aside width="200px" style="background-color: rgb(238, 241, 246)">

      <el-menu router :default-openeds="['index']" >
        <el-submenu   index="index" >
          <template slot="title"><i class="el-icon-tickets"></i>功能选择
          </template>
          <el-menu-item  index="/function/deposit">存款</el-menu-item>
          <el-menu-item index="/function/withdrawals">取款</el-menu-item>
          <el-menu-item index="/function/transfer">转账</el-menu-item>
          <el-menu-item index="/function/inquire">查询</el-menu-item>
          <el-menu-item index="/function/record">记录</el-menu-item>
        </el-submenu>
      </el-menu>
    </el-aside>

    <el-container>
      <el-header style="text-align: right; font-size: 20px" >
        <el-dropdown>
          <i class="el-icon-setting" style="margin-right: 25px" ></i>
          <el-dropdown-menu slot="dropdown">
            <el-dropdown-item @click.native="confirmExit" >退出登录</el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
        <span>{{name}}</span>
      </el-header>

      <el-main>
        <router-view></router-view>
      </el-main>
    </el-container>
  </el-container>
</template>

<style>
  html,body{
    margin: 0;
    padding: 0;
  }

  .el-header {
    background-color: #B3C0D1;
    color: #333;
    line-height: 60px;
  }

  .el-aside {
    color: #333;
  }
</style>

<script>
  export default {


    data() {
      return {
        name:''
      }
    },
      created() {
        this.name=sessionStorage.getItem('name');
      },
      methods: {
          confirmExit() {
              this.$confirm('确认要退出登录吗？?', '提示', {
                  confirmButtonText: '确定',
                  cancelButtonText: '取消',
                  type: 'warning'
              }).then(() => {
                  sessionStorage.removeItem('token');
                  sessionStorage.removeItem('name');
                  this.$router.push('/');
              }).catch(() => {
              });
          }
      }
  }
</script>
