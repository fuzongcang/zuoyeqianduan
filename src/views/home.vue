<template>
<el-container style="height: 500px; border: 1px solid #eee">
  <el-aside width="200px" style="background-color: rgb(238, 241, 246)">
    <el-menu :default-openeds="['1', '3']" :unique-opened="true" :router="true">
      <el-submenu index="1" v-for="(item,index) in rootMenu" :key="item.menuId">
        <template slot="title">
         <i class="el-icon-menu"></i>
           <span>{{item.menuName}}</span>
           
        </template>
        <el-menu-item-group>
        <template slot="title" v-for="(submenu,i) in menu.filter(s => s.parentId == item.menuId)">
           <el-menu-item :index="mentitem.linkUrl" v-for="(mentitem,subindex) in menu.filter(s => s.parentId == submenu.menuId)" 
            :key="mentitem.menuId">
              <span>{{mentitem.menuName}}</span>
           </el-menu-item>
        </template>
         </el-menu-item-group>
      
      </el-submenu>
    </el-menu>
  </el-aside>
  
  <el-container>
    <el-header style="text-align: right; font-size: 12px">
      <el-dropdown>
        <i class="el-icon-setting" style="margin-right: 15px"></i>
        <el-dropdown-menu slot="dropdown">
          <el-link @click="TIO">退出登录</el-link>
        </el-dropdown-menu>
      </el-dropdown>
      <span>管理员</span>
    </el-header>
    
    <el-main>
      <router-view/>
    </el-main>
  </el-container>
</el-container>
</template>

<style>
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
      const item = {
        date: '2016-05-02',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1518 弄'
      };
      return {
        tableData: Array(20).fill(item),
        menu:[]
      }
    },
    created(){
      this.$axios.get('https://localhost:44340/api/VUE/Show')
      .then(res =>{
        this.menu = res.data;
      })
    },
    computed:{
      rootMenu(){
        return this.menu.filter(s => s.parentId == 0)
      }
    },
    methods:{
        TIO(){
            this.$confirm('确定要退出登录吗, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$router.push('/login');
        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消'
          });          
        });
            
        }
    }
  };
</script>