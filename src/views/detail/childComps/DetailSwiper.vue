<template>
  <swiper class="swiper">
    <swiper-item v-for="(item, index) in images">
      <img :src="item" alt="">
    </swiper-item>
  </swiper>
</template>

<script>
  import {Swiper, SwiperItem} from 'components/common/swiper'
  export default {
    name: "DetailSwiper",
    components: {
      Swiper,
      SwiperItem,
    },
    props: {
      images: Array,
      default(){
        return []
      }
    }
  }
</script>

<style scoped>
.swiper {
  height: 300px;
  overflow: hidden;
}
</style>