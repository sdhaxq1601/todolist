<template>
<div>
    <label for="title">ToDoList</label>
    <input type="text" name="title" id="title" placeholder="text..." v-on:keyup.enter="handler">
    <!-- <div>
        <ul>
            <li v-for="(list,index) in lists" :key="index">{{list.title}}</li>
        </ul>
    </div> -->
    <co-a v-for="(list,index) in lists" :key="index" :list="list" :index="index" @delete="lists.splice($event,1)"></co-a>
    <input type="button" value="clear" @click="lists=[]">
</div>
</template>
<script>
let coA={
    data(){
        return{}
    },
     props:[
      'list',
      'index'
  ],
    template:`<div><span>{{list.title+''+index}}</span><button @click="$emit('delete',index)">cancel</button></div>`
}
export default {
  name: "Home",
  components:{"co-a":coA},
 
  data() {
    return {
      lists: []
    };
  },
  methods: {
    handler: function(e) {
      const list = {
        done: false,
        title: e.target.value
      };
      this.lists.push(list);
      e.target.value = "";
      console.log(this.lists);
    }
  }
};
</script>
<style>
input{
    width:20vw;
}
</style>
