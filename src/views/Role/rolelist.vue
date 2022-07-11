<template>
<div>
  <el-button type="primary" @click="dialogFormVisible = true">添加</el-button>
  <el-table
    :data="tableData"
    style="width: 100%">
    <el-table-column
      prop="roleId"
      label="角色Id"
      width="180">
    </el-table-column>
     <el-table-column
      prop="roleName"
      label="角色名称"
      width="180">
    </el-table-column>
    <el-table-column label="操作">
      <template slot-scope="scope">
        <el-button
          size="mini"
          @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
          <el-button
          size="mini"
          @click="Distribution(scope.row.roleId)">分配权限</el-button>
        <el-button
          size="mini"
          type="danger"
          @click="handleDelete(scope.$index, scope.row)">删除</el-button>
      </template>
    </el-table-column>
  </el-table>
  <el-dialog title="菜单" :visible.sync="dialogTableVisible">
    <menuTree ref="dialogTree" />
    <div slot="footer" class="dialog-footer">
    <el-button @click="dialogTableVisible = false">关 闭</el-button>
    <el-button type="primary" @click="SavePermission">确 定</el-button>
  </div>
  </el-dialog>

  <el-dialog title="角色新增" :visible.sync="dialogFormVisible">
    <RoleAddPro @addTable="addTable" />
</el-dialog>
  </div>
</template>

<script>
import menuTree from '@/views/Role/tree.vue'
import RoleAddPro from '@/views/Role/roleadd.vue'
  export default {
    components: {
    menuTree,
    RoleAddPro
    },
    data() {
      return {
        tableData: [],
        dialogTableVisible:false,
        dialogFormVisible:false,
        MenuRole:{
          roleId:0,
          menuId:[]
        }
      }
    },
    methods: {
      handleEdit(index, row) {
        console.log(index, row);
      },
      handleDelete(index, row) {
        this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$axios.delete('https://localhost:44340/api/Role/Del?id='+row.roleId)
          .then(res =>{
            if(res.data > 0)
            {
              this.$message.success('删除成功');
              this.GetShow();
            }
          })
        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消删除'
          });          
        });
      },
      Distribution(roleId){
        this.MenuRole.roleId = roleId;
        this.dialogTableVisible = true;
      },
      GetShow(){
        this.$axios.get('https://localhost:44340/api/Role/Show')
        .then(res =>{
          
            this.tableData = res.data;
            debugger
        })
      },
      SavePermission(){
        this.MenuRole.menuId= this.$refs["dialogTree"].$refs["menuTree"].getCheckedNodes(true,true).map(m=>m.value);
        this.$axios.post('https://localhost:44340/api/Role/SavePermission',this.MenuRole)
        .then(res =>{
             this.$message({
                message: '分配成功',
                type: 'success',
                onClose:(m=>{
                  this.GetShow();
                })
              });
        })
      }
    },
    created(){
        this.GetShow();
    }
  }
</script>
