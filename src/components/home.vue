<template>
    <el-container class="home-container">
      <!-- 头部区域 -->
    <el-header>Header<el-button type="info" @click="logout">退出</el-button></el-header>
    <el-container>
      <!-- 侧边栏 -->
      <el-aside width="200px">
         <el-menu :default-openeds="['1', '3']">
      <el-submenu :index="item.id + ''" v-for="item in menuList" :key="menuList[item]">
        <template slot="title"><i class="el-icon-message"></i><span>{{item.authName}}</span></template>
        <el-menu-item index="1-4-1" v-for="stem in item.children" :key="item.children[stem]">
           <i class="el-icon-message"></i>
            <span>{{stem.authName}}</span>
        </el-menu-item>
      </el-submenu>
    </el-menu>
        Aside
      </el-aside>
      <!-- 右侧内容主体 -->
      <el-main>Main</el-main>
    </el-container>
  </el-container>
</template>
<script>
export default {
  name: "home",
  data(){
    return {
      menuList : []
    }
  },
  created(){
    this. getMenuList()
  },
  methods: {
    logout() {
      window.sessionStorage.clear();
      this.$router.push("/login");
    },
   async getMenuList(){
      const {data: res} = await this.$http.get('menus')

      this.menuList =res.data
    }
  }
};
</script>
<style lang="stylus" scoped>
.home-container{
  height 100%
  .el-header{
    display flex
    justify-content space-between
    padding-left 0
    align-items center
    background-color: #373d41
  }
  .el-aside{
    background-color: #333744
  }
  .el-main{
     background-color: #eaedf1
  }
}
</style>