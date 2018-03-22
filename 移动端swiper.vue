<template lang="pug">
  .rr-swiper
    rr-swiper-item(v-for="(i, idx) in '210564'.split('')",
      :key="idx"
      :idx="idx"
      :style="computedStyle(idx)") {{i}}
</template>

<script>
/**
 * @constructor 移动端touch事件手势处理
 */
export default {
  name: 'rr-swiper',
  data () {
    return {
      domEl: HTMLElement,
      startX: 0,
      moveX: 0,
      currentIdx: 2,
      pageWidth: 0,
      thresholdWidth: 0,
      animated: true
    }
  },
  props: {
    threshold: {
      type: Number,
      default: 0.3
    }
  },
  methods: {
    /**
     * @method checkWidthThreshold 检查滑动阈值 超过阈值翻页 未超过不翻页
     * @var { Number } thresholdWidth 阈值 根据该值判断滑动是否需要改变idx
     * @var { Number } pageWidth 页面宽度
     * @var { Number } threshold 阈值比例 0 ~ 1
     */
    computedThreshold: function () {
      this.thresholdWidth = this.pageWidth * this.threshold
    },
    checkedAddorReduce: function () {
      if ((this.moveX > 0 && this.currentIdx === 5) || (this.moveX < 0 && this.currentIdx === 0)) return
      this.moveX > 0 ? this.currentIdx++ : this.currentIdx--
    },
    computedStyle: function (idx) {
      return {
        transform: `translateX(${(this.pageWidth * (idx - this.currentIdx) - this.moveX)}px)`,
        background: 'red',
        transition: this.animated ? 'all .3s' : ''
      }
    },
    initTouchStart: function (e) {
      this.startX = e.touches[0].clientX
      this.animated = false
    },
    initTouchMove: function (e) {
      this.moveX = this.startX - e.touches[0].clientX
      this.$emit('touchmove', this.moveX)
    },
    initTouchEnd: function (e) {
      if (Math.abs(this.moveX) > this.thresholdWidth) this.checkedAddorReduce()
      this.moveX = 0
      this.$emit('touchend')
      this.animated = true
    },
    initTouchEvent: function () {
      this.$el.addEventListener('touchstart', this.initTouchStart)
      this.$el.addEventListener('touchmove', this.initTouchMove)
      this.$el.addEventListener('touchend', this.initTouchEnd)
    }
  },
  mounted () {
    this.computedThreshold()
    this.$nextTick(() => {
      this.initTouchEvent()
    })
  },
  created () {
    this.pageWidth = document.body.clientWidth || document.documentElement.clientWidth
  }
}
</script>

<style lang="sass" scoped>
.rr-swiper
  position: relative;
  height: 200px;
  overflow: hidden;
</style>
