<template>
    <div>
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
  methods: {},
  computed: {},
  watch: {}
}
</script>
<style scoped>
div {
  width: 60%;
}
span {
  width: 75%;
  display:block;
  text-align: start;
  float:left;
}
input {
  width:5%;
  float:left;
}
button {
  width: 15%;
  float: right;
}
</style>
