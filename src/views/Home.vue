<template lang="pug">
  div(:class="$style.container")
    div(@click="start", v-if="!started") 开始
    div(v-if="started") 总时间剩余： {{min}} : {{sec}}
    div(v-if="subStarted") 本次有效胎动剩余： {{subMin}} : {{subSec}}
    div(@click="movement", v-if="started") 动一下
    div(v-if="started") 已动{{movementArr.length}}次
</template>
<script>
  export default {
    data() {
      return {
        started: false,
        subStarted: false,
        timer: null,
        startTime: null,
        subStartTime: null,
        min: '60',
        sec: '00',
        subMin: '5',
        subSec: '00',
        movementArr: []
      }
    },
    methods: {
      start() {
        this.startTime = new Date()
        this.started = true
        this.timer = setInterval(() => {
          if (!this.started) clearInterval(this.timer)
          if (+this.sec > 0) {
            this.sec = +this.sec - 1
          } else {
            if (+this.min > 0) {
              this.min = +this.min - 1
              this.sec = '59'
            } else {
              this.started = false
              alert(`动了${this.movementArr.length}次`)
            }
          }
          if (this.subStarted) {
            if (+this.subSec > 0) {
              this.subSec = +this.subSec - 1
            } else {
              if (+this.subMin > 0) {
                this.subMin = +this.subMin - 1
                this.subSec = '59'
              } else {
                this.subStarted = false
                alert(`动了${this.movementArr.length}次`)
              }
            }
          }
        }, 1000)
      },
      movement() {
        if(this.subStarted) return
        const {min, sec} = this
        this.subStarted = true
        this.movementArr.push(min + ':' + sec)

      }
    }
  }
</script>
<style lang="stylus" module>
  .container
    padding 0
</style>
