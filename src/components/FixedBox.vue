<template>
    <div>
        <div
          :id="id"
          :style="css"
          v-on:touchstart="dosth"
          v-on:touchmove="move"
          v-on:touchend="slide"
          ></div>
    </div>
</template>
<script>
export default {
  name: 'FixedBox',
  data () {
    return {
      id:"fix",
      left: 100,
      top: 100,
      x: 0,
      y: 0,
      ox: 0,
      oy: 0,
      ot: 0
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
      this.x = this.ox = e.touches[0].screenX
      this.y = this.oy = e.touches[0].screenY
      this.ot = new Date().getTime()
    },
    move (e) {
      e.preventDefault()
      this.top += e.touches[0].screenY - this.y
      this.left += e.touches[0].screenX - this.x
      this.x = e.touches[0].screenX
      this.y = e.touches[0].screenY
    },
    slide (e) {
      const startSlideTime = new Date().getTime()
      let totalSlideTime=0 // ms
      const dt = (startSlideTime - this.ot) / 1000
      const dx = this.x - this.ox
      const dy = this.y - this.oy
      this.ox = this.x
      this.oy = this.y
      const dd = Math.sqrt(dx*dx+dy*dy)
      let g = -100 // 加速度
      let gx = dx / dd * g // x加速度
      let gy = dy / dd * g // y加速度
      let sx = dx / dt
      let sy = dy / dt
      let ix = 1
      let iy = 1
      const duraTime = (((0 - sx) / gx) || ((0 - sy) / gy)) * 1000 //
      console.log(duraTime)
      const that = this
      const time = 1 / 60
      const h=this.$el.querySelector('#' + that.id).clientHeight
      const w=that.$el.querySelector('#' + that.id).clientWidth
      console.log(dx, dy, sx, sy)
      if (dt > 0.1 && (dx || dy)) {
      const Iid=setInterval(function(){
        // console.log("interval",totalSlideTime,duraTime)
        if (totalSlideTime >= duraTime) {
          clearInterval(Iid)
          return
          }
        // console.log(that.top,that.left,gy,gx)
        totalSlideTime += time * 1000
        that.x = (sx * time + gx * time * time / 2) * ix
        that.y = (sy * time + gy * time * time / 2) * iy
        that.left += that.x
        that.top += that.y
        if(that.top<=0 ||
        that.top + h>=window.innerHeight){
          iy=-iy
        }
        if(that.left<=0 ||
        that.left + w>=window.innerWidth){
          ix=-ix
        }
        sx = gx * time + sx
        sy = gy * time + sy
      },
      time * 1000
      )
      }
    }
  }
}
</script>
