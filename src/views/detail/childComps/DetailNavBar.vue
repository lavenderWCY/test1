<template>
  <nav-bar>
    <img slot="left" class="back" @click="backClick" src="~assets/img/common/back.svg" alt="">

    <div slot="center" class="title">
      <div class="title-item"
           v-for="(item, index) in title"
           :class="{active: currentIndex === index}"
           @click="titleClick(index)"
      >
        {{item}}
      </div>
    </div>
  </nav-bar>
</template>

<script>
  import NavBar from 'components/common/navbar/NavBar'
  export default {
    name: "DetailNavBar",
    data(){
      return{
        title:['商品','参数','评论','推荐'],
        currentIndex: 0
      }
    },
    components: {
      NavBar,
    },
    methods: {
      backClick(){
        this.$router.back()
      },
      titleClick(index){
        this.currentIndex = index
        this.$emit('titleClick', index)
      }
    }
  }
</script>

<style scoped>
.title {
  display: flex;
  padding: 0 20px;
}
  .title-item {
    flex: 1;
    font-size: 14px;
  }
  .back {
    margin-top: 12px;
  }
  .active {
    color: var(--color-high-text);
  }
</style>