<template>
  <swiper>
    <swiper-item v-for="item in banners" @load="swiperLoad">
      <a :href="item.link">
        <img :src="item.image" alt="">
      </a>
    </swiper-item>
  </swiper>
</template>

<script>
  import {Swiper,SwiperItem} from 'components/common/swiper'
  export default {
    name: "HomeSwiper",
    props: {
      banners: {
        type: Array,
        default() {
          return []
        }
      }
    },
    components: {
      Swiper,
      SwiperItem,
    },
    methods: {
      swiperLoad() {
        this.$emit('swiperLoadFinish')
      }
    }
  }
</script>

<style scoped>

</style>