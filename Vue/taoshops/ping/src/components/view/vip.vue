<template>
  <section style="">
    <div style="position:absolute;background:#ffffff">
        <div style="position:fixed;z-index:999;width:100%">
          <van-cell-group>
            <van-search v-model="keyWord" show-action @search="superSearch"  placeholder="搜索您要搜索的宝贝">
              <van-button slot="action" @click="superSearch" type="danger" size="small">搜索</van-button>
            </van-search>
          </van-cell-group>
          <van-cell-group>
            <div style="text-align:center;">
            <van-row>
              <van-col span="8">
                <van-button type="primary" size='small' style="width:100%">仅看有券</van-button>
              </van-col>
              <van-col span="8">
                <van-button type="danger" size='small' style="width:100%">海外商品</van-button>
              </van-col>
              <van-col span="8">
                <van-button type="primary" size='small' style="width:100%">站内精选</van-button>
              </van-col>
            </van-row>
            </div>
          </van-cell-group>
        </div> 
    </div>
    <section style="height:76px;"></section>
    <section>
      <van-pull-refresh v-model.lazy="isLoading" @refresh="onRefresh">
          <div>
            <!-- 无数据的情况 -->
          <div v-if='rowlength==0'>
            <img src="../../assets/icon/icon_kong.png" class="goods-imgurl"/>
              <div style="text-align:center">暂无数据哦...</div>
          </div>
            <!-- list列表 -->
          <div v-else>
              <van-list> 
                <van-row>
                  <div v-for="(r, key) in articles" :key="key" @click="JumpPageDetails(r.productId)">
                    <div v-if="(key+1)%2==1">
                      <van-col span="12" class="img_border" >
                        <van-cell-group>
                          <img :src="r.itempic" class="goods-imgurl">
                            <div class="good_name" style="height:0.9rem;">
                              <img :src="r.icon"/>{{r.title}}</div>
                              <div>
                                <span>
                                  <van-tag type="danger" v-if="r.qfee!=0">{{r.qfee}}元优惠券</van-tag>
                                </span>
                                  <van-tag plain class="intergral_style" style="color: #fa2509;">约赚:{{r.fl}} 佣金币</van-tag>
                              </div>
                              <div style="height:0.8rem">
                                <span class="price_style">￥{{r.itemfee2}}</span>
                                <span v-if="r.qfee!=0" class="goods-express">&nbsp;原价:￥{{r.itemfee}}</span>
                                <span class="salenumber_style">已售{{r.itemmsell}}件</span>                       
                              </div>
                        </van-cell-group>
                      </van-col>
                    </div>
                    <div v-else>
                      <van-cell-group>
                          <van-col span="12" class="img_border" >
                              <van-cell-group>
                                  <img :src="r.itempic" class="goods-imgurl">
                                      <div class="good_name" style="height:0.9rem;">
                                        <img :src="r.icon"/>{{r.title}}</div>
                                      <div>
                                        <span>
                                          <van-tag type="danger" v-if="r.qfee!=0">{{r.qfee}}元优惠券</van-tag>
                                        </span>
                                        <van-tag plain  class="intergral_style" style="color: #fa2509;">约赚:{{r.fl}} 佣金币</van-tag>
                                      </div>
                                      <div style="height:0.8rem">
                                        <span class="price_style">￥{{r.itemfee2}}</span>
                                        <span v-if="r.qfee!=0" class="goods-express">&nbsp;原价:￥{{r.itemfee}}</span>
                                        <span class="salenumber_style">已售{{r.itemmsell}}件</span>
                                      </div>
                              </van-cell-group>                                    
                          </van-col>
                        </van-cell-group>
                      </div>
                  </div>  
                </van-row>
                <div style="text-align:center;font-size:16px;background:#ffffff;color:#999;">{{messages}}</div>
              </van-list>
          </div>
          </div>
      </van-pull-refresh>  
    </section>
    <section style="height:50px;"></section>
    <!-- 底部标签 -->
    <div>
      <van-row>
        <van-goods-action>
            <van-goods-action-mini-btn style="width:25%;" @click="JumpIndex">
                <div style="text-align:center;">
                  <img src="../../assets/icon/icons_index.png" style="width:0.7rem;"/>
                  <div><span style="font-size:14px;">首页</span></div>
                </div>
            </van-goods-action-mini-btn>
             <van-goods-action-mini-btn style="width:25%;" @click="JumpVip">
              <div style="text-align:center;">
                  <img src="../../assets/icon/icon_search_current.png" style="width:0.7rem;"/>
                   <div><span style="font-size:14px;">超级搜</span></div>
              </div>
            </van-goods-action-mini-btn>
            <van-goods-action-mini-btn style="width:25%;" @click="JumpLove">
              <div style="text-align:center;">
                  <img src="../../assets/icon/icons_love.png" style="width:0.7rem;"/>
                  <div><span style="font-size:14px;">收藏</span></div>
              </div>
            </van-goods-action-mini-btn>
            <van-goods-action-mini-btn style="width:25%;" @click="JumpUser">
                <div style="text-align:center;">
                  <img src="../../assets/icon/icons_my.png" style="width:0.7rem;"/>
                  <div><span style="font-size:14px;">我的</span></div>
                </div>
            </van-goods-action-mini-btn>
        </van-goods-action>
      </van-row>
    </div>
  </section>
</template>
<script>
import notice from "../../assets/icon/icon_notices.png";
const API_PROXY = "https://bird.ioliu.cn/v1/?url=";
export default {
  data() {
    return {
      url: "http://shg.blpev.cn:8080/shg-api/api/",
      url2:
        "http://pupumei.cn/app/index.php?c=entry&i=2&m=bsht_tbkquan&do=gethdinfo&itemid=&sid=&itemfee=&itempic=&nb_shopid=",
      keyWord: "",
      rowlength: "",
      isLoading: true,
      articles: {},
      messages: "",
      limit: 1,
      pagesize: 20,
      onlyq: 0, //1只看有券，0全部
      isht: 0, //1海外商品 0 全部
      issite: 0 //1 站内精选 0 全部
    };
  },
  mounted() {
    this.superSearch();
  },
  methods: {
    superSearch() {
      // console.log("onlyqchecked:" + this.onlyqchecked);
      // console.log("ishtchecked:" + this.ishtchecked);
      // console.log("issitechecked:" + this.issitechecked);
      let _this = this;
      // 此处使用node做了代理
      this.$axios
        .post(
          _this.url2 +
            "&q=" +
            _this.keyWord +
            "&limit=" +
            _this.limit +
            "&size=" +
            _this.pagesize +
            "&onlyq=" +
            _this.onlyq +
            "&isht=" +
            _this.isht +
            "&issite=" +
            _this.issite +
            "&shopid=&actid=&type=5&nbnice=1&openid="
        )
        .then(function(response) {
          // 将得到的数据放到vue中的data
          _this.articles = response.data.nb_qlist;
          var lengths = response.data.nb_qlist.length;
          _this.rowlength = lengths;
          _this.limit += 1;
          console.log(_this.limit)
        })
        .catch(function(error) {
          console.log(error);
        });
      window.addEventListener("scroll", this.searchfunction);
    },
    searchfunction() {
      let _this = this;
      let sw = true;
      var a =
        window.innerHeight ||
        document.documentElement.clientHeight ||
        document.body.clientHeight;
      var b =
        document.documentElement.scrollTop == 0
          ? document.body.scrollTop
          : document.documentElement.scrollTop;
      var c =
        document.documentElement.scrollTop == 0
          ? document.body.scrollHeight
          : document.documentElement.scrollHeight;
      if (a + Math.floor(b) == c || a + Math.ceil(b) == c) {
        //如果开关打开则加载数据
        if (sw == true) {
          // 将开关关闭
          sw = false;
          _this.$axios
            .get(
              _this.url2 +
                "&q=" +
                _this.keyWord +
                "&limit=" +
                _this.limit++ +
                "&size=" +
                _this.pagesize +
                "&onlyq=" +
                _this.onlyq +
                "&isht=" +
                _this.isht +
                "&issite=" +
                _this.issite +
                "&shopid=&actid=&type=5&nbnice=1&openid="
            )
            .then(function(response) {
              // 将新获取的数据push到vue中的data，就会反应到视图中了
              var lengths = response.data.nb_qlist.length;
              for (var i = 0; i < lengths; i++) {
                _this.articles.push(response.data.nb_qlist[i]);
              }
              // console.log(lengths)
              // 数据更新完毕，将开关打开
              sw = true;
              console.log(_this.limit)
            })
            .catch(function(error) {
              console.log(error);
              _this.$toast("网络异常错误...");
            });
        }
        if (sw == false) {
          _this.messages = "正在加载中...";
        }
      }
    },
    // 跳转商品详情页
    JumpPageDetails(productId) {
      var goodsId = productId;
      this.$router.push({
        path: "/ping",
        name: "PageDetails",
        query: {
          goodsId: goodsId
        },
        params: {
          data: this.value
        }
      });
    },
    JumpLove() {
      this.$router.push({
        path: "/ping",
        name: "love"
      });
    },
    JumpVip() {
      this.$router.push({
        path: "/ping",
        name: "vip"
      });
    },
    JumpIndex() {
      this.$router.push({
        path: "/ping",
        name: "indexs"
      });
    },
    JumpUser() {
      this.$router.push({
        path: "/ping",
        name: "user"
      });
    },
    onRefresh() {
      // 下拉刷新
      setTimeout(() => {
        this.$toast("刷新成功");
        this.isLoading = false;
      }, 500);
    }
  },
  destroyed() {
    window.removeEventListener("scroll", this.searchfunction);
  }
};
</script>
<style>
body {
  background: #f1f1f1;
}
.good_name {
  text-overflow: -o-ellipsis-lastline;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  font-size: 0.3rem;
}
.goods-imgurl {
  width: 100%;
  height: 100%;
  margin: auto;
}
.img_border {
  border: 0.05rem solid #f1f1f1;
}

.intergral_style {
  color: #fa2509;
  font-size: 0.3rem;
}

.price_style {
  color: red;
  font-size: 0.5rem;
}

.goods-express {
  color: #999;
  font-size: 0.3rem;
}

.salenumber_style {
  color: #999;
  font-size: 0.2rem;
}
</style>
