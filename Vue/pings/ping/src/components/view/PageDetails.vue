<template>
  <div>
      <!-- <h2>{{goodsId}}</h2> -->
     <!--轮播图片-->
    <div>
      <van-swipe class="goods-swipe" :autoplay="4000">
        <van-swipe-item v-for="(img, index) in images" :key="index">
        <img :src="img" style="height:10.0rem;">
        </van-swipe-item>
      </van-swipe>
    </div>
    <!-- 价格 -->
    <div style="background:#ffffff;">
      <div style="height:0.2rem;">&nbsp;</div>
      <div style="margin:0.1rem;">
        <div>
          <div style="float:left;">
          <span style="font-size:0.5rem;color:red;">{{articles.normalCouponAfterPrice}}元</span> 
          <span v-if="articles.hasCoupon==true" style="text-decoration:line-through;color:#999">￥{{articles.minNormalPrice}}</span> 
          <van-tag type="danger" v-if="articles.hasQuanfan==true">云联全返</van-tag>
          </div>
          <div style="text-align:right;color:#999;">
          已拼{{articles.salesNum}}件
          </div>
          <div style="clear:both;"></div>
          </div>
        <div style="font-size:0.4rem;">{{articles.goodsName}}</div>
        <div>
          <div style="float:left;">
          <van-tag type="danger" plain v-if="articles.hasCoupon==true">{{articles.couponPrice}}元优惠券</van-tag>
          </div>
          <div v-if="articles.hasCoupon==true" style="text-align:right;color:#999;">
          <span>剩余{{articles.couponSurplus}}张</span>
          <span>到期{{articles.couponEndTime}}</span>
          </div>
          <div style="clear:both;"></div>
        </div>
      </div>
    </div>

    <!-- 奖励 -->
    <div style="background:#ffffff">
      <div style="">
        <div>
         <van-cell-group>
          <van-cell is-link value="奖励说明" @click="jumpmessage" >
            <template slot="title">
              <span style="color:#f05a03">约奖：{{articles.integral}}白积分</span>
            </template>
          </van-cell>
          <van-cell is-link value="升级拼团客多奖30%">
            <template slot="title">
              <span style="color:#f05a03">约奖：{{articles.vipIntegral}}白积分</span>
            </template>
          </van-cell>
        </van-cell-group>
        </div>
      </div>
    </div>
    <div style="height:0.1rem;">&nbsp;</div>
    <div style="background:#ffffff;">
      <div style="margin:0rem 0.1rem 0rem 0.1rem">
        <p style="text-align:center;">商品详细参数</p>
        <div>{{articles.goodsDesc}}</div>
      </div>
    </div>
    <div style="background:#ffffff;height:60px;">&nbsp;</div>
    <!-- 底部菜单 -->
    <div>
      <van-goods-action>
        <van-goods-action-mini-btn icon="chat" text="帮助" @click="jumphelp"/>
        <van-goods-action-mini-btn icon="cart" text="收藏夹" />
        <van-goods-action-big-btn text="分享赚积分"/>
        <van-goods-action-big-btn text="去参团" primary />
      </van-goods-action>
    </div>

    <van-actionsheet v-model="helpshow" title="奖励说明">
      <p style="font-size:16px;">1、该奖励为约奖，指通过优惠价购买一件商品的积分奖励，多买多得（根据实际付款金额），实际到帐积分有上下微小浮动属于正常现象。</p>
      <p style="font-size:16px;">2、必须通过拼团客系统进入直接拼团付款才有奖励，进入后先收藏，再通过多多收藏下单是无奖励的，如需收藏，请使用拼团客系统的收藏夹。</p> 
    </van-actionsheet>

    <van-popup v-model="bottomhelpshow" position="right">
      <div style="width:200px;height:400px;backgroud:#ffffff;">内容</div>
    </van-popup>
  </div>
</template>
<script>
export default {
  data() {
    return {
      goodsId: "",
      articles: {},
      images: {},
      helpshow: false,
      bottomhelpshow:false
    };
  },
  mounted() {
    this.getParams();
    // 缓存指针
    let _this = this;
    // 此处使用node做了代理
    this.$axios
      .get("http://ptk.baolinzhe.com/ptk/api/v1/product/" + _this.goodsId)
      .then(function(response) {
        // console.log(JSON.parse(response.data).stories);
        // 将得到的数据放到vue中的data
        _this.articles = response.data.result;
        _this.images = response.data.result.images;
        console.log(_this.articles);
        // console.log(_this.images);
        // console.log(response.data.result.goodsName);
      })
      .catch(function(error) {
        console.log(error);
      });
  },
  methods: {
    getParams() {
      // 取到路由带过来的参数
      var routerParams = this.$route.params.goodsId;
      // 将数据放在当前组件的数据内
      this.goodsId = routerParams;
    },
    jumpmessage() {
      this.helpshow = true;
    },
    jumphelp(){
      this.bottomhelpshow=true;
    }
  },
  watch: {
    // 监测路由变化,只要变化了就调用获取路由参数方法将数据存储本组件即可
    $route: "getParams"
  }
};
</script>
<style lang="less">
.goods {
  &-swipe {
    img {
      width: 100%;
      height: 100%;
      margin: auto;
      display: block;
      background-size: contain;
    }
  }
}
body {
  background: #f1f1f1;
}
.con_img {
  position: relative;
}
.ms {
  position: absolute;
  top: 20px;
  left: 15px;
}
</style>


