<template>
  <div>
    <nav-bar class="nav-bar"><div slot="center">购物车({{cartLength}})</div></nav-bar>

  </div>
</template>

<script>
  import NavBar from 'components/common/navbar/NavBar'
  export default {
    name: "Cart",
    components:{
      NavBar,
    }
  }
</script>

<style scoped>

</style>
