<template>
    <div class="a-todo" draggable="true" @dragover="allowDrag">
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
    allowDrag (ev) {
      ev.preventDefault()
      // console.log(ev)
      return this.$emit('myDragover', ev)
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
  width: 15%;
  float: right;
}
</style>
