<template>
    <div class="a-todo"
      :class="{bg:todo.done}"
      draggable="true"
      @dragstart="dragStart"
      @drop="drop"
      @dragover="allowDrag">
        <input type="checkbox" v-model="todo.done" v-on:change="$emit('gl',index)">
        <span>{{todo.title}}</span>
        <button v-on:click="$emit('delete',index)">X</button>
        <div style="clear:both;"></div>
    </div>
</template>
<script>
export default {
  name: 'ATodo',
  props: ['todo', 'index'],
  data () {
    return {}
  },
  methods: {
    dragStart (ev) {
      ev.dataTransfer.setData('index', this.index)
      // console.log(ev)
    },
    allowDrag (ev) {
      ev.preventDefault()
      // console.log(ev)
      return this.$emit('myDragover', ev)
    },
    drop (ev) {
      ev.preventDefault()
      const data = ev.dataTransfer.getData('index')
      this.$emit('willDragOrder', [data, this.index])
    }
  },
  computed: {},
  watch: {}
}
</script>
<style scoped>
.a-todo {
  width: 60%;
  border: 1px solid #666;
  margin-bottom: 5px;
  margin-left: auto;
  margin-right: auto;
}
span {
  width: 75%;
  display:block;
  text-align: start;
  float:left;
}
input {
  width:5%;
  height: 15px;
  float:left;
}
button {
  float: right;
}
.bg{
  background-color: #898;
}
</style>
