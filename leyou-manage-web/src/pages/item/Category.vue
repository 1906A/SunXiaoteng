<template>
  <v-card>
      <v-flex xs12 sm10>
        <v-tree url="/item/category/list"
                :isEdit="isEdit"
                @handleAdd="handleAdd"
                @handleEdit="handleEdit"
                @handleDelete="handleDelete"
                @handleClick="handleClick"
        />
      </v-flex>
  </v-card>
</template>

<script>

  export default {
    name: "category",
    data() {
      return {
        isEdit:true,
        //treeData:treeData
      }
    },
    methods: {
     /* handleAdd(node) {
        console.log("add .... ");
        console.log(node);
        this.$http.post('item/category/add',node)
          .then((res)=>{
            alert(res.data);
          })

      },*/
     /* handleEdit(node) {



      },*/
      /*handleDelete(id) {
        console.log("delete ... " + id)



      },*/
     /* handleClick(node) {

      }*/
    }
  };
</script>

<style scoped>

</style>
