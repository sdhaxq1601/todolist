<template>
    <div>
        <div
          :style="css"
          v-on:touchstart="dosth"
          v-on:touchmove="move"
          ></div>
    </div>
</template>
<script>
export default {
  name: 'FixedBox',
  data () {
    return {
      left: 100,
      top: 100,
      x: 0,
      y: 0
    }
  },
  computed: {
    css () {
      return `background:#f9e;width:10vw;height:10vw;position:fixed;top:${this.top}px;left:${
        this.left
      }px;`
    }
  },
  methods: {
    dosth (e) {
      this.x = e.touches[0].screenX
      this.y = e.touches[0].screenY
    },
    move (e) {
      e.preventDefault()
      this.top += e.touches[0].screenY - this.y
      this.left += e.touches[0].screenX - this.x
      this.x = e.touches[0].screenX
      this.y = e.touches[0].screenY
    }
  }
}
</script>
