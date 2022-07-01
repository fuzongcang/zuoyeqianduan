<template>
<div>
<el-tree
  :data="data"
  show-checkbox
  node-key="id"
  :default-expanded-keys="[2, 3]"
  :default-checked-keys="[5]"
  :props="defaultProps">
</el-tree>
</div>
</template>
<script>
  export default {
    data() {
      return {
        data: [],
        defaultProps: {
          value: 'value',
          label: 'label'
        }
      };
    },
    created(){
        this.$axios.get('https://localhost:44340/api/VUE/GetList')
        .then(res =>{
            var reg = new RegExp('\\,"children":\\[]',"g");
        this.data = JSON.parse(JSON.stringify(res.data).replace(reg,""));
        })
     }
  }
</script>