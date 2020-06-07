<template>
  <v-list class="pt-0 pb-0" dense>
    <TreeItem
      class="item" :model="model" v-for="(model, index) in db" :key="index"
      :url="url"
      :isEdit="isEdit"
      :nodes="nodes"
      @handleAdd="handleAdd"
      @handleEdit="handleEdit"
      @handleDelete="handleDelete"
      @handleClick="handleClick"
    />
  </v-list>
</template>

<script>
  import TreeItem from './TreeItem';

  export default {
    name: "vTree",
    props: {
      url: String,
      isEdit: {
        type: Boolean,
        default: false
      },
      treeData:{
        type:Array
      }
    },
    data() {
      return {
        db: [],
        nodes:{
          opened:null,
          selected:{isSelected:false}
        }
      }
    },
    components: {
      TreeItem
    },
    created() {
      if(this.treeData && this.treeData.length > 0){
        this.db = this.treeData;
        return;
      }
      this.getData();
    },
    methods: {
      getData() {
        this.$http.get(this.url, {params: {pid: 0}}).then(resp => {
          this.db = resp.data;
          this.db.forEach(n => n['path'] = [n.name])
        })
      },
      handleAdd(node) {
      console.log("add .... ");
        console.log(node);
        alert(node)
        this.$http.post('item/category/add',node)
          .then((res)=>{
            alert(res.data);
          })

      },
      handleEdit(node) {
        console.log("------------------------------");
        console.log(node);
        console.log("------------------------------");

        let id = node.id;
        alert("id:"+id);
        if (id ==0){
          this.$http.post('item/category/add',node)
            .then((res)=>{
              alert(res.data);
              if (res.data == 'SUCC'){
                alert("新增成功");
              }else if (res.data='FAIL'){
                alert("新增失败")
              }
            }).catch((error)=>{
            alert("请求失败");
          })
        }else {
          //修改
          this.$http.post('/item/category/update',node).
          then((res)=>{
            alert(res.data);
            if (res.data='SUCC'){
              alert("修改成功");
              window.location.reload();
            }else if (res.data='FAIL'){
              alert("修改失败");
            }
            window.location.reload();
          }).catch((err)=>{
            alert("请求失败")
          })
        }
      },
      handleDelete(id) {
        console.log(node);
        this.$http.get('item/category/delete?id='+id,node)
          .then((res)=>{
            if (res.data =='SUCC') {
              alert("删除成功");
            }else if (res.data=='FAIL'){
              alert("删除失败");
            }
          }).catch((err)=>{
          alert("请求失败")
        })
      },
      handleClick(node){
        this.$emit("handleClick", this.copyNodeInfo(node))
      },
      // 根据id删除
      deleteById(id, arr) {
        let src = arr || this.db;
        for (let i in src) {
          let d = src[i];
          if (d.id === id) {
            src.splice(i, 1)
            return;
          }
          if (d.children && d.children.length > 0) {
            this.deleteById(id, d.children)
          }
        }
      },
      copyNodeInfo(node){
        const o = {};
        for(let i in node){
          o[i] = node[i];
        }
        return o;
      }
    },
    watch: {}
  }
</script>

<style scoped>
  .item {
    cursor: pointer;
  }
</style>
