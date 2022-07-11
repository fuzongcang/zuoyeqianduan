<template>
<el-form ref="form" :model="form" label-width="80px">
  <el-form-item label="角色名称">
    <el-input v-model="form.roleName"></el-input>
  </el-form-item>
  <el-form-item>
    <el-button type="primary" @click="onSubmit">立即创建</el-button>
    <el-button>取消</el-button>
  </el-form-item>
</el-form>
</template>
<script>
  export default {
    data() {
      return {
        form: {
          roleName:''
        }
      }
    },
    methods: {
      onSubmit() {
        this.$axios.post('https://localhost:44340/api/Role/Add',this.form)
          .then(res =>{
              this.$message.success('添加成功');
              this.$emit("addTable",true);
          })
      }
    }
  }
</script>