<template>
  <div class="recommend">
    <div v-for="item in recommends" class="recommenditem">
      <a :href="item.link">
        <img :src="item.image" alt="">
        <div>{{item.title}}</div>
      </a>
    </div>
  </div>
</template>

<script>
  export default {
    name: "RecommendView",
    props: {
      recommends: {
        type: Array,
        default() {
          return []
        }
      }
    }
  }
</script>

<style scoped>
 .recommend{
   display: flex;
   width: 100%;
   text-align: center;
   font-size: 12px;
   padding: 10px 0 20px;
   border-bottom: 8px solid #eeee;
 }
  .recommenditem {
    width: 25%;
  }
  .recommenditem img {
    width: 65px;
    height: 65px;
  }
</style>