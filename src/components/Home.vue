<template>
<div>
    <label for="title">ToDoList</label>
    <input type="text" name="title" id="title" placeholder="text..." v-on:keyup.enter="handler">
    <!-- <div>
        <ul>
            <li v-for="(list,index) in lists" :key="index">{{list.title}}</li>
        </ul>
    </div> -->
    <p>正在进行...<span class='bold'>{{lists.filter(i=>!i.done).length}}</span></p>
    <ATodo
      v-for="(list,index) in lists"
      :key="'a'+index" :todo="list"
      :index="index"
      v-show="!list.done"
      @delete="lists.splice($event,1)"
      @willDragOrder="dragOrder"></ATodo>
    <p>已经完成！<span class='bold'>{{lists.filter(i=>i.done).length}}</span></p>
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
    handler (e) {
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
    splicePush (i) {
      const c = this.lists.splice(i, 1)
      this.lists.push(c[0])
    },
    dragOrder (e) {
      const [o, d] = e
      if (o === d) {
        return
      }
      const c = this.lists.splice(o, 1)
      const r = this.lists.splice(d)
      const f = this.lists
      this.lists = [...f, ...c, ...r]
    }
  },
  computed: {

  },
  watch: {

  }
}
</script>
<style>
.bold{
  font-weight: bold
}
</style>
