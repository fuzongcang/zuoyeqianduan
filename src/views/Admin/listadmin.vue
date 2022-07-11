<template>
<div>
  <el-button type="primary" @click="dialogTableVisible=true">添加</el-button>
  <el-table
    :data="tableData"
    style="width: 100%">
    <el-table-column
      prop="userName"
      label="账号"
      width="180">
    </el-table-column>
     <el-table-column
      prop="email"
      label="邮箱"
      width="180">
    </el-table-column>
    <el-table-column
      label="登录时间"
      width="180">
       <template slot-scope="scope">
        <i class="el-icon-time"></i>
        <span style="margin-left: 10px">{{ fromdate(scope.row.createTime) }}</span>
      </template>
    </el-table-column>
    <el-table-column label="操作">
      <template slot-scope="scope">
        <el-button
          size="mini"
          @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
        <el-button
          size="mini"
          type="danger"
          @click="handleDelete(scope.$index, scope.row)">删除</el-button>
      </template>
    </el-table-column>
  </el-table>
   <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="page.pindex"
      :page-sizes="[1, 2, 3, 4]"
      :page-size="page.psize"
      layout="total, sizes, prev, pager, next, jumper"
      :total="page.total">
    </el-pagination>
    <el-dialog title="新增账号" :visible.sync="dialogTableVisible">
      <addpro :key="new Date().getTime()" />
    </el-dialog>
</div>
</template>

<script>
import addpro from '@/views/Admin/adminadd.vue'
  export default {
    components: {
    addpro
  },
    data() {
      return {
        tableData: [],
        page:{
            pindex:1,
            psize:2,
            total:0
        },
        dialogTableVisible:false
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
          this.$axios.post('https://localhost:44340/api/Admins/Shan?id='+row.adminId)
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
      handleSizeChange(val){
        this.page.psize = val;
         this.page.pindex = 1;
        this.GetShow();
      },
      handleCurrentChange(val){
        this.page.pindex = val;
        this.GetShow();
      },
      fromdate(){
        var date = new Date();
        var list = (date.getFullYear())+"-"+(date.getMonth()+1)+"-"+(date.getDate());
        return list;
      },
      GetShow(){
        var PageL = {
            pindex :this.page.pindex,
            psize:this.page.psize
        }
        this.$axios.get('https://localhost:44340/api/Admins/Page',{params:PageL})
        .then(res =>{
            this.tableData = res.data.item1;
            this.page.total = res.data.item2;
        })
      }
    },
    created(){
        this.GetShow();
    }
  }
</script>