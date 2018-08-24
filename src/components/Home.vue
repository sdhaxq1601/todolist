<template>
<div>
    <label for="title">ToDoList</label>
    <input type="text" name="title" id="title" placeholder="text..." v-on:keyup.enter="handler">
    <!-- <div>
        <ul>
            <li v-for="(list,index) in lists" :key="index">{{list.title}}</li>
        </ul>
    </div> -->
    <ATodo v-for="(list,index) in lists" :key="index" :todo="list" :index="index" @delete="lists.splice($event,1)"></ATodo>
    <input type="button" value="clear" v-show="lists.length!=0" @click="lists=[]">
</div>
</template>
<script>
//1234
import ATodo from "./ATodo";
import ADone from "./ADone";

export default {
  name: "Home", 
  components:{
      ATodo,
      ADone
  },
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
