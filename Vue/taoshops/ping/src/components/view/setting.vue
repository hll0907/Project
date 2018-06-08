<template>
  <section style="background:#ffffff;">
    <section>
     <div style="height:25px;font-size:14px;"><span style="margin:10px;">头像与昵称</span></div>
    </section>
    <section style="background:#ffffff;">
      <van-card :title="nickname" :thumb="imageURL">
        <div slot="footer">
          <!-- <van-button @click="Jumpsynchronization">同步微信头像昵称</van-button> -->
        </div>
      </van-card>
    </section>
      <section style="height:5px;background:#f1f1f1;"></section>
    <section>
      <van-cell-group>
        <div> 
            <van-row>
              <van-col span="12">
                <van-cell>
                  <template slot="title">
                    <span>手机号</span>
                  </template>
                </van-cell>
              </van-col>
              <div @click="JumpBindingPhone">
              <van-col span="12">
                <div style="text-align:right;">
                 <van-cell is-link>
                  <template slot="title">
                    <span v-if="phone==''" @click="JumpBindingPhone">去绑定手机号</span>
                    <span v-else>{{phone}}</span>
                  </template>
                </van-cell>
                </div>
              </van-col>
              </div>
            </van-row>
        </div>
      </van-cell-group>
    </section>
    <section>
      <van-cell-group>
            <div> 
            <van-row>
              <van-col span="12">
                <van-cell>
                  <template slot="title">
                    <span>支付宝账号</span>
                  </template>
                </van-cell>
              </van-col>
              <div @click="JumpBindingWeixin">
              <van-col span="12">
                <div style="text-align:right;">
                 <van-cell is-link>
                  <template slot="title">
                    <span v-if="weixinnumber==''" @click="JumpBindingWeixin">暂无支付宝账号</span>
                    <span v-else>{{weixinnumber}}</span>
                  </template>
                </van-cell>
                </div>
              </van-col>
              </div>
            </van-row>
        </div>
      </van-cell-group>
    </section>
    <section style="height:10px;background:#f1f1f1;"></section>
    <section>
      <van-cell-group>
            <div> 
            <van-row>
              <van-col span="12">
                <van-cell>
                  <template slot="title">
                    <span>微信收款二维码</span>
                  </template>
                </van-cell>
              </van-col>
              <div @click="JumpBindingWeixinmoneyScan">
              <van-col span="12">
                <div style="text-align:right;">
                 <van-cell is-link>
                  <template slot="title">
                    <span v-if="wxMoneyQrcode!=''">已添加</span>
                    <span v-else>未添加</span>
                  </template>
                </van-cell>
                </div>
              </van-col>
              </div>
            </van-row>
        </div>
      </van-cell-group>
    </section>
    
  </section>
</template>
<script>
import icon_nickname from "../../assets/icon/icon_head.png";
import fansVue from "./fans.vue";

export default {
  data() {
    return {
      id: 790714,
      url: "http://shg.yuf2.cn:8080/shg-api/api/",
      phone: "",
      nickname: "",
      weixinnumber: "",
      imageURL: icon_nickname,
      wxMoneyQrcode: "",
      wxQrcode: ""
    };
  },
  mounted() {
    // this.id = sessionStorage.getItem("userId");
    // var dataJson = JSON.parse(
    //   decodeURIComponent(this.cookies.getCookie("userData"))
    // );
    // this.id = dataJson.id;
    this.getUserData();
  },
  methods: {
    getUserData() {
      let _this = this;
      if (_this.id == "") {
        _this.$toast("当前您还未登录哦");
      } else {
        // 此处使用node做了代理
        this.$axios
          .get(_this.url + "/user/" + _this.id)
          .then(function(response) {
            // 将得到的数据放到vue中的data
            _this.userdata = response.data.result;
            _this.nickname = _this.userdata.nickName;
            _this.phone = _this.userdata.phone;
            _this.weixinnumber = _this.userdata.alipay;
            _this.wxMoneyQrcode = _this.userdata.wxMoneyQrcode;
            if (_this.userdata.headPic == "") {
              _this.imageURL = _this.imageURL;
            } else {
              _this.imageURL = _this.userdata.imageUrl;
            }
            // console.log(_this.userdata);
            // console.log(_this.wxMoneyQrcode);
            // console.log(_this.wxQrcode);
          })
          .catch(function(error) {
            console.log(error);
            _this.$toast("网络异常错误...");
          });
      }
    },
    JumpBindingPhone() {
      this.$router.push({
        path: "/shg",
        name: "bindingnumber",
        params: {
          phone: this.phone,
        }
      });
    },
    JumpBindingWeixin() {
      this.$router.push({
        path: "/shg",
        name: "bindingalipay",
        params: {
          weixinnumber: this.weixinnumber
        }
      });
    },
    JumpBindingWeixinmoneyScan() {
      this.$router.push({
        path: "/shg",
        name: "bindingmoneyscan"
      });
    }
  }
};
</script>

<style>
</style>