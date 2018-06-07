<template>
    <section>
 <!--轮播图片-->
    <div>
      <van-swipe class="goods-swipe" :autoplay="4000" v-if="code==1">
        <van-swipe-item v-for="(img, index) in images" :key="index">
        <img :src="img" style="height:10.0rem;">
        </van-swipe-item>
      </van-swipe>
      <van-swipe class="goods-swipe" :autoplay="4000" v-else>
        <img src="../../assets/icon/icon_scan.png" style="height:10.0rem;">
      </van-swipe>
    </div>
    <!-- 价格 -->
    <div style="background:#ffffff;">
      <div style="height:0.2rem;">&nbsp;</div>
      <div style="margin:0.1rem;">
        <div>
          <div style="float:left;">
          <span style="font-size:0.5rem;color:red;">{{price}}元</span> 
          <span v-if="couponPrice!=0" style="text-decoration:line-through;color:#999">￥{{originPrice}}</span> 
          </div>
          <div style="text-align:right;color:#999;">
          已售{{salesNum}}件
          </div>
          <div style="clear:both;"></div>
          </div>
        <div style="font-size:0.4rem;">
         {{articles.title}}</div>
        <div>
          <div style="float:left;">
          <van-tag type="danger" plain v-if="couponPrice!=0">{{couponPrice}}元优惠券</van-tag>
          </div>
          <div v-if="couponPrice!=0" style="text-align:right;color:#999;">
          <span>剩余{{itemyhj_yl}}张</span>
          <span>到期{{itemyhj_etime}}</span>
          </div>
          <div style="clear:both;"></div>
        </div>
      </div>
    </div>
    <!-- 奖励 -->
    <div style="background:#ffffff">
      <div>
        <div>
         <van-cell-group>
          <van-cell is-link value="奖励说明"  >
            <template slot="title">
              <span style="color:#f05a03">约赚：{{integral}}佣金币</span>
            </template>
          </van-cell>
        </van-cell-group>
        </div>
      </div>
    </div>
    <div style="background:#ffffff">
      <div>
        <div>
         <van-cell-group>
          <van-cell is-link value="查看商品详情"  @click="Jumpdetailsurl">
          </van-cell>
        </van-cell-group>
        </div>
      </div>
    </div>
    <div style="background:#ffffff">
      <div>
        <div>
         <van-cell-group>
          <van-cell :value="subtitle">
          </van-cell>
        </van-cell-group>
        </div>
      </div>
    </div>
    <div style="height:1px;background:#c0c0c0"></div>
    <img :src="itempic" style="width:100%;height:100%"/>
    <div style="height:50px;"></div>
    <div>
      <van-goods-action>
       <van-goods-action-mini-btn @click="JumpAddCollect">
          <div style="text-align:center;">
            <van-icon name="e619"/>
            <div>加入收藏夹</div>
          </div>
        </van-goods-action-mini-btn>
        <van-goods-action-mini-btn @click="JumpDelCollect(goodsId)">
          <div style="text-align:center;color:red;">
             <van-icon name="e619"/>
            <div>取消收藏</div>
          </div>
        </van-goods-action-mini-btn>
        <van-goods-action-big-btn text="立即购买" primary @click="onClickbuy"/>
      </van-goods-action>
    </div>
    </section>
</template>
<script>
export default {
  data() {
    return {
      url: "http://shg.blpev.cn:8080/shg-api/api/",
      goodsId: "",
      articles: {},
      images: {},
      detailImages: {},
      code: 0,
      imgurl: "",
      moduleDescUrl: "",
      price: 10,
      originPrice: 100,
      couponPrice: 10,
      itempic: "",
      integral: "",
      salesNum: 10,
      itemyhj_url: "",
      itemyhj_yl: "",
      itemyhj_etime: "",
      subtitle: "",
      actid: "",
      maijia_id: "",
      couponUrl: "",
      transferchainUrl: "",
      shopName: ""
    };
  },
  mounted() {
    var keyword = window.location.href;
    var i = keyword.indexOf("Id=");
    this.goodsId = decodeURI(
      keyword.substring(i + 3, keyword.lastIndexOf("&qfee"))
    );
    var couponPrices = keyword.indexOf("&qfee=");
    this.couponPrice = decodeURI(
      keyword.substring(couponPrices + 6, keyword.lastIndexOf("&itempic"))
    );
    this.itempic = decodeURIComponent(
      keyword.substring(
        keyword.indexOf("&itempic=") + 9,
        keyword.lastIndexOf("&fl=")
      )
    );
    this.integral = decodeURI(
      keyword.substring(
        keyword.indexOf("&fl=") + 4,
        keyword.lastIndexOf("&itemfee2")
      )
    );
    this.price = decodeURI(
      keyword.substring(
        keyword.indexOf("&itemfee2=") + 10,
        keyword.lastIndexOf("&itemfee")
      )
    );
    this.originPrice = decodeURI(
      keyword.substring(
        keyword.indexOf("&itemfee=") + 9,
        keyword.lastIndexOf("&itemmsell")
      )
    );
    this.salesNum = decodeURI(
      keyword.substring(
        keyword.indexOf("&itemmsell=") + 11,
        keyword.lastIndexOf("&itemyhj_url")
      )
    );
    this.itemyhj_url = decodeURI(
      keyword.substring(
        keyword.indexOf("&itemyhj_url=") + 13,
        keyword.lastIndexOf("&itemyhj_yl")
      )
    );
    this.itemyhj_yl = decodeURI(
      keyword.substring(
        keyword.indexOf("&itemyhj_yl=") + 13,
        keyword.lastIndexOf("&itemyhj_etime")
      )
    );
    this.itemyhj_etime = decodeURI(
      keyword.substring(
        keyword.indexOf("&itemyhj_etime=") + 15,
        keyword.lastIndexOf("&actid")
      )
    );
    this.actid = decodeURI(
      keyword.substring(
        keyword.indexOf("&actid=") + 7,
        keyword.lastIndexOf("&maijia_id")
      )
    );
    this.maijia_id = decodeURI(
      keyword.substring(keyword.indexOf("&maijia_id=") + 11, keyword.length)
    );
    this.couponUrl =
      "https://uland.taobao.com/quan/detail?sellerId=" +
      this.maijia_id +
      "&activityId=" +
      this.actid;
    let _this = this;
    this.$axios
      .get(
        "/api?&api=mtop.taobao.detail.getdetail&v=6.0&ttid=2013%40taobao_h5_1.0.0&type=jsonp&dataType=jsonp&data=%7B%22itemNumId%22%3A%22" +
          _this.goodsId +
          "%22%7D"
      )
      .then(function(response) {
        _this.articles = response.data.data.item;
        _this.images = _this.articles.images;
        _this.code = 1;
        _this.moduleDescUrl = _this.articles.h5moduleDescUrl;
        _this.subtitle = _this.articles.subtitle;
        _this.shopName = _this.articles.title;
        alert(_this.shopName);
      })
      .catch(function(error) {
        console.log(error);
      });
    this.getConvertUrl();
  },
  methods: {
    Jumpdetailsurl() {
      window.location.href = this.moduleDescUrl;
    },
    onClickbuy() {
      this.getConvertUrl();
      this.maketaobaocommand();
    },
    getConvertUrl() {
      let _this = this;
      alert(_this.couponUrl);
      this.$axios
        .post(
          _this.url +
            "/product/convert_url?productId=" +
            _this.goodsId +
            "&couponUrl=" +
            _this.couponUrl
        )
        .then(function(response) {
          if (response.data.code == 1) {
            _this.transferchainUrl = response.data.result;
            console.log(_this.transferchainUrl);
          }
        })
        .catch(function(error) {
          console.log(error);
        });
    },
    //生成淘宝口令
    maketaobaocommand() {
      let _this = this;
      this.$axios
        .get(
          _this.url +
            "/product/tpwd?text=" +
            _this.shopName +
            "&url=" +
            _this.transferchainUrl +
            "&logoUrl=" +
            _this.itempic
        )
        .then(function(response) {
          if (response.data.code == 1) {
            console.log(response.data.result);
          }
        })
        .catch(function(error) {
          console.log(error);
        });
    }
  }
};
</script>

