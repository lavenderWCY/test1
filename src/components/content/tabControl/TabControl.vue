<template>
  <div class="tabcontrol">
    <div v-for="(item,index) in title" class="tab-control-item" :class="{active: index === currentIndex}" @click="itemClick(index)">
      <span >{{item}}</span>
    </div>
  </div>
</template>

<script>
  export default {
    name: "TabControl",
    props: {
      title: {
        type: Array,
        default() {
          return []
        }
      }
    },
    data() {
      return {
        currentIndex: 0
      }
    },
    methods: {
      itemClick(index) {
        this.currentIndex = index
        this.$emit('tabclick', index)
      }
    }
  }
</script>

<style scoped>
  .tabcontrol {
    display: flex;
    text-align: center;
   font-size: 15px;
    height: 40px;
    line-height: 40px;
    background-color: #fff;
    z-index: 9;
  }
.tab-control-item {
    flex: 1;
}
  .tabcontrol span {
    padding: 5px;
  }
  .active {
    color: var(--color-high-text);
  }
  .active span {
    border-bottom: 3px solid var(--color-tint);
  }
</style>