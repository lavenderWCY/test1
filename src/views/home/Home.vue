<template>
  <div id="home">
    <nav-bar class="home-nav"> <div slot="center">购物街</div></nav-bar>
    <tab-control class="tab-control" v-show="isShowBackTop" :title="['流行','新款','精品']" />
    <scroll class="content"
            ref="scroll"
            :probe-type="3"
            @scroll="contentscroll"
            :pull-up-load="true"
            @pullingUp="loadMore"
            @swiperLoadFinish="swiperLoadFinish"
    >
      <home-swiper :banners="banners"/>
      <recomment-view :recommends="recommends"/>
      <feature-view/>
      <tab-control class="tab-control"  :title="['流行','新款','精品']" @tabclick="tabclick"/>
      <goods-list :goods="goods[currentType].list"/>

    </scroll>
    <back-top @click.native="backclick" v-show="isShowBackTop" ref="tabControl"/>

  </div>
</template>

<script>

  import HomeSwiper from './childComps/HomeSwiper'
  import RecommentView from './childComps/RecommendView'
  import FeatureView from './childComps/FeatureView'

  import NavBar from 'components/common/navbar/NavBar'
  import TabControl from 'components/content/tabControl/TabControl'
  import GoodsList from 'components/content/goods/GoodsList'
  import Scroll from 'components/common/scroll/Scroll'
  import BackTop from 'components/content/backtop/BackTop'


  import {getHomeMultidata, getHomeGoods, } from "network/home";



  export default {
    name: "Home",
    data() {
      return {
        banners: [],
        recommends: [],
        goods: {
          'pop': {page: 0, list: []},
          'new': {page: 0, list: []},
          'sell': {page: 0, list: []},
        },
        currentType: 'pop',
        isShowBackTop: false,
        taboffsetTop: 0,
        showTabControl: false
      }
    },
    components: {
      HomeSwiper,
      RecommentView,
      FeatureView,
      NavBar,
      TabControl,
      GoodsList,
      Scroll,
      BackTop,
    },
    created() {
      //请求多个数据
     this.getHomeMultidata()

      //请求商品数据
      this.getHomeGoods('pop')
      this.getHomeGoods('new')
      this.getHomeGoods('sell')
    },
    mounted(){
      //建通item中图片加载完
      this.$bus.$on('imgLoadFinish', ()=>{
        this.$refs.scroll.refresh()
      })
    },
    methods: {
      tabclick(index){
        switch (index) {
          case 0: this.currentType ='pop';break;
          case 1: this.currentType ='new';break;
          case 2: this.currentType ='sell';break;
        }
      },
      backclick(){
       // this.$refs.scroll.scroll.scrollTo(0,0,500)
        this.$refs.scroll.scrollTo(0,0)
      },
      contentscroll(position){
       // console.log(position)
       //  if ( (-position.y) > 1000) {
       //    this.isShowBackTop = true
       //  }
        this.isShowBackTop = (-position.y)> 1000

        this.showTabControl = position.y <= -this.taboffsetTop
      },
      loadMore(){
        //console.log('上拉')
        //加载
        this.getHomeGoods(this.currentType)

        //刷新
        return this.$refs.scroll.scroll.refresh()
      },
      swiperLoadFinish(){
        //console.log(this.$refs.tabControl.$el.offsetTop)
        this.taboffsetTop = this.$refs.tabControl.$el.offsetTop
      },


      /*网络请求*/
      getHomeMultidata() {
        getHomeMultidata().then(res => {
       /*   console.log(res)*/
          this.banners = res.data.banner.list;
          this.recommends = res.data.recommend.list;
        });
      },
      getHomeGoods(type){
       const page = this.goods[type].page +1 ;
        getHomeGoods(type, page).then(res=> {
        /*  console.log(res)*/
          this.goods[type].list.push(...res.data.list)
          this.goods[type].page += 1

         this.$refs.scroll.scroll.finishPullUp()
        })
      },
    }
  }
</script>

<style scoped>
  #home {
    height: 100vh;
    position: relative;
  }
  .home-nav {
    background-color: var(--color-tint);
    color: white;
    position: relative;
    /*
    position: fixed;
    left: 0;
    right: 0;
    top: 0;
   */
    z-index: 9;
  }
  .tab-control {
   /* position: sticky;
    top: 44px;*/
    position: relative;
    z-index: 9;
  }
  .content {
    position: absolute;
    left: 0;
    right: 0;
    top: 44px;
    bottom: 49px;
  }
</style>
