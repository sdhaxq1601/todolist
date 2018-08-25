<template>
<div>
    <label for="title">ToDoList</label>
    <input type="text" name="title" id="title" placeholder="text..." v-on:keyup.enter="handler">
    <!-- <div>
        <ul>
            <li v-for="(list,index) in lists" :key="index">{{list.title}}</li>
        </ul>
    </div> -->
    <p>正在进行...{{lists.filter(i=>!i.done).length}}</p>
    <ATodo
      v-for="(list,index) in lists"
      :key="'a'+index" :todo="list"
      :index="index"
      v-show="!list.done"
      @delete="lists.splice($event,1)"
      @myDragover="allowDrag"></ATodo>
    <p>已经完成！{{lists.filter(i=>i.done).length}}</p>
    <ATodo v-for="(list,index) in lists" :key="'b'+index" :todo="list" :index="index" v-show="list.done" @delete="lists.splice($event,1)" @gl="splicePush($event)"></ATodo>
    <input type="button" value="clear" v-show="lists.length!=0" @click="lists=[]">
    <div>{{test}}</div>
</div>
</template>
<script>
// ++
import ATodo from './ATodo'
import ADone from './ADone'

export default {
  name: 'Home',
  components: {
    ATodo,
    ADone
  },
  data () {
    return {
      lists: [],
      test: ''
    }
  },
  methods: {
    handler: function (e) {
      const list = {
        done: false,
        title: e.target.value
      }
      this.lists.unshift(list)
      e.target.value = ''
      // console.log(this.lists);
    },
    allowDrag (ev) {
      // ev.preventDefault()
      // console.log(ev)
      if (this.lists.length > 5) {
        this.lists.pop()
      }
    },
    splicePush: function (i) {
      const c = this.lists.splice(i, 1)
      this.lists.push(c[0])
    }
  },
  computed: {

  },
  watch: {

  }
}
</script>
<style>
input{
    width:20vw;
}
</style>
