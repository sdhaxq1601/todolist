<template>
<div class='test'>
    <div id='d1' @click="click"></div>
    <div id='d2' @click.native="click"></div>
</div>
</template>
<script>
export default {
    name: 'test',
    data: function(){
        return {}
    },
    methods: {
        click: function(e){
            console.log(e)
        }
    }
}
</script>
<style>
.test div{
    height:20px;
    width:20px;
    border:1px solid #999;
}
</style>

