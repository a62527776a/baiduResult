<template lang="pug">
  .rr-swiper-item
    slot
</template>

<script>
export default {
  name: 'rr-swiper-item',
  props: {
    idx: {
      type: Number,
      default: 0
    }
  },
  data () {
    return {

    }
  },
  methods: {

  },
  created () {
    console.log(this.idx)
  }
}
</script>

<style lang="sass" scoped>
.rr-swiper-item
  position: absolute;
  width: 100%;
  background: red;
</style>
