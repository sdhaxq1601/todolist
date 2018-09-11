<template>
    <div>
        <div
          :id="id"
          :style="css"
          v-on:touchstart="dosth"
          v-on:touchmove="move"
          v-on:touchend="slide"
          >
          <svg width="100%" height="100%"><circle cx="50%" cy="50%" r="50%" stroke="black" stroke-width="0" fill="red"></circle></svg>
          </div>
    </div>
</template>
<script>
export default {
  name: 'FixedBox',
  data () {
    return {
      id: "fix",
      points: new Array(5).fill(null),
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
      return `width:10vw;height:10vw;position:fixed;top:${this.top}px;left:${this.left}px;`
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
      let point = {}
      this.x = point['x'] = e.touches[0].screenX
      this.y = point['y']  = e.touches[0].screenY
      point['time']  = e.timeStamp
      this.points.shift()
      this.points.push(point)
    },
    slide (e) {
      const startSlideTime = new Date().getTime()
      let totalSlideTime=0 // ms
      if (this.points[0]===null) {return}
      // const dt = (startSlideTime - this.ot) / 1000
      const dt = (this.points[4].time - this.points[0].time) / 1000
      // const dy = this.y - this.oy
      // const dx = this.x - this.ox
      const dy = this.points[4].y - this.points[0].y
      const dx = this.points[4].x - this.points[0].x
      console.log(this.points.map(i=>i.x),dt,dy,dx)
      this.ox = this.x
      this.oy = this.y
      const dd = Math.sqrt(dx*dx+dy*dy)
      let g = -800 // 加速度
      let gx = dx / dd * g // x加速度
      let gy = dy / dd * g // y加速度
      let sx = dx / dt
      let sy = dy / dt
      let ix = 1
      let iy = 1
      const duraTime = (((0 - sx) / gx) || ((0 - sy) / gy)) * 1000 //
      console.log(duraTime)
      const h=this.$el.querySelector('#' + this.id).clientHeight
      const w=this.$el.querySelector('#' + this.id).clientWidth
      const tempTop = this.top
      const tempLeft = this.left
      this.top = this.top >= 0?(this.top+h)>window.innerHeight?window.innerHeight-h:this.top:0
      this.left = this.left >= 0?(this.left+w)>window.innerWidth?window.innerWidth-w:this.left:0
      if (this.top !== tempTop) { iy=-iy }
      if (this.left !== tempLeft) { ix=-ix }
      const time = 1 / 60
      const that = this
      console.log(dx, dy, sx, sy)
      if (dt > 0.01 && (dx || dy)) {
      const Iid=setInterval(function(){
        // console.log("interval",totalSlideTime,duraTime)
        if (totalSlideTime >= duraTime) {
          clearInterval(Iid)
          return
          }
        // console.log(that.top,that.left,gy,gx)
        if(totalSlideTime===0){console.log(that.left,that.top)}
        totalSlideTime += time * 1000
        that.x = (sx * time + gx * time * time / 2) * ix
        that.y = (sy * time + gy * time * time / 2) * iy
        that.left += that.x
        that.top += that.y
        if((that.top<=0?(that.top=0) || true:false) ||
        ((that.top + h>=window.innerHeight)?(that.top=window.innerHeight-w)||true:false)){
          iy=-iy
        }
        if((that.left<=0?(that.left=0) || true:false) ||
        ((that.left + w>=window.innerWidth)?(that.left=window.innerWidth-w)||true:false)){
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
