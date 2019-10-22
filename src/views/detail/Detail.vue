<template>
  <div class="detail">
    <detail-nav-bar class="nav-bar" @titleClick="titleClick"/>
    <scroll class="content"
            ref="scroll">
      <detail-swiper :images="topImages"/>
      <detail-base-info :goods="goods"/>
      <detail-shop-info :shop="shop"/>
      <detail-goods-info :detailInfo="detailInfo" @imageLoad="imageLoad"/>
      <detail-param-info ref="params" :paramInfo="paramInfo"/>
      <detail-comment-info ref="comment" :commentInfo="commentInfo"/>
      <goods-list ref="recommend" :goodsItem="recommends"/>
    </scroll>
    <detail-bottom-bar></detail-bottom-bar>

  </div>
</template>

<script>
  import DetailNavBar from './childComps/DetailNavBar'
  import DetailSwiper from './childComps/DetailSwiper'
  import DetailBaseInfo from './childComps/DetailBaseInfo'
  import DetailShopInfo from './childComps/DetailShopInfo'
  import DetailGoodsInfo from './childComps/DetailGoodsInfo'
  import DetailParamInfo from './childComps/DetailParamInfo'
  import DetailCommentInfo from './childComps/DetailCommentInfo'
  import DetailBottomBar from './childComps/DetailBottomBar'



  import GoodsList from 'components/content/goods/GoodsList'

  import Scroll from 'components/common/scroll/Scroll'

  import {getDetail, Goods, Shop,GoodsParam ,getRecommend} from 'network/detail'

  export default {
    name: "Detail",
    components:{
      DetailNavBar,
      DetailSwiper,
      getDetail,
      DetailBaseInfo,
      DetailShopInfo,
      Scroll,
      DetailGoodsInfo,
      DetailParamInfo,
      DetailCommentInfo,
      GoodsList,
      DetailBottomBar,
    },
    data(){
      return{
        iid: null,
        topImages:[],
        goods: {},
        shop: {},
        detailInfo:{},
        paramInfo: {},
        commentInfo: {},
        recommends:[],
        themeTopYs: []
    }
    },
    methods: {
      imageLoad(){
        this.$refs.scroll.refresh()

        this.themeTopYs = []
        this.themeTopYs.push(0)
        this.themeTopYs.push(this.$refs.params.$el.offsetTop)
        this.themeTopYs.push(this.$refs.comment.$el.offsetTop)
        this.themeTopYs.push(this.$refs.recommend.$el.offsetTop)

        console.log(this.themeTopYs)
      },
      titleClick(index){
        this.$refs.scroll.scrollTo(0, -this.themeTopYs[index], 200)
      },

    },

    created() {

      //请求id
      this.iid = this.$route.query.iid
      //this.iid = this.$route.params.iid

      //请求商品
      getDetail(this.iid).then(res=>{
        //获取图片
        console.log(res)
        const data = res.result
       this.topImages = res.result.itemInfo.topImages

        //获取商品基本信息
        this.goods = new Goods(data.itemInfo, data.columns, data.shopInfo.services)

        //商铺信息
        this.shop = new Shop(data.shopInfo)

        //保存商品详情数据
        this.detailInfo = data.detailInfo;

        //详情
        this.paramInfo = new GoodsParam(data.itemParams.info, data.itemParams.rule)

        //评论信息(评论信息有为0的情况/判断)
        if(data.rate.cRate !== 0) {
          this.commentInfo = data.rate.list[0];
        }
      })

      //请求推荐数据
      getRecommend().then(res=>{
        console.log(res)
        this.recommends = res.data.list
      })

    }
  }
</script>

<style scoped>
.detail{
  position: relative;
  z-index: 9;
  background-color: #fff;
  height: 100vh;
}
  .nav-bar {
    position: relative;
    z-index: 9;
    background-color: #fff;
  }
  .content {
    height: calc(100% - 44);
    background-color: #fff;
  }
</style>