<template>
<!-- 提现 -->
  <section>
      <section>
        <van-notice-bar :text="nitice" :left-icon="notice_icon"/>
        <van-cell-group>
         <van-cell title="可提现佣金币余额" />
        </van-cell-group>
        <van-cell-group style="height:2.0rem;">
        <van-cell>
            <template slot="title">
                <div style="text-align:center;font-size:26px;margin-top:0.4rem;">
                  <van-icon name="e610" style="color:#ffd600;"/>
                  <span>{{money}}</span>
                  <div style="color:#999;font-size:12px;">(100佣金币=1元)</div>
                </div>
            </template>
        </van-cell>
        </van-cell-group>
      </section>
      <section style="height:10px;"></section>
      <section>
        <van-cell-group>
            <van-field center v-model="number" label="佣金币" placeholder="请输入佣金币数量" type="number" v-on:input="myFocus">
            <div slot="button" style="color:#d81e06;" @click="JumpTotalMoney">全部提现
            </div>
            </van-field>
           <van-cell-group>
           <van-row>
              <van-col span="12">
                <van-cell>
                  <template slot="title">
                    <span>微信收钱码</span>
                  </template>
                </van-cell>
              </van-col>
              <div>
              <van-col span="12">   
                <div style="text-align:right;" v-if="wxMoneyQrcode=='?time='+times">
                 <van-cell is-link @click="JumpBindingMoneysacn">
                  <template slot="title">
                    <span>未添加</span>
                  </template>
                </van-cell>
                </div>
                 <div style="text-align:right;" v-else  @click="JumpMoneyShowScan">
                 <van-cell>
                  <template slot="title">
                    <span ><img :src="wxMoneyQrcode" style="width:10%;height:0.4rem;" /></span>
                  </template>
                </van-cell>
                </div>
              </van-col>
              </div>
            </van-row>
            </van-cell-group>
            <van-cell-group>
            <van-row>
              <van-col span="12">
                <van-cell>
                  <template slot="title">
                    <span>支付宝账号</span>
                  </template>
                </van-cell>
              </van-col>
              <div>
              <van-col span="12">
                <div style="text-align:right;">
                 <van-cell>
                  <template slot="title">
                    <span v-if="alipay==''">未添加</span>
                    <span>{{alipay}}</span>
                  </template>
                </van-cell>
                </div>
              </van-col>
              </div>
            </van-row>
        </van-cell-group>
        <van-cell-group>
            <van-row>
              <van-col span="12">
                <van-cell>
                  <template slot="title">
                    <span>云联惠账号</span>
                  </template>
                </van-cell>
              </van-col>
              <div>
              <van-col span="12">
                <div style="text-align:right;">
                 <van-cell>
                  <template slot="title">
                    <span v-if="ylhId==''">未添加</span>
                    <span>{{ylhId}}</span>
                  </template>
                </van-cell>
                </div>
              </van-col>
              </div>
            </van-row>
        </van-cell-group>
        <van-cell-group>
            <van-row>
              <van-col span="12">
                <van-cell>
                  <template slot="title">
                    <span>选择提现方式</span>
                  </template>
                </van-cell>
              </van-col>
              <div>
              <van-col span="12">
                <div>
                  <van-radio-group v-model="radio" @change="onchange">
                    <van-radio name="支付宝">支付宝</van-radio>
                    <van-radio name="微信">微信</van-radio>
                    <van-radio name="云联惠" disabled >云联惠</van-radio>
                                        <!-- <van-radio name="云联惠"  >云联惠</van-radio> -->
                  </van-radio-group>
                </div>
              </van-col>
              </div>
            </van-row>
        </van-cell-group>
            <div style="height:0.6rem;margin-top:5px;">
              <span style="font-size:16px;margin:15px;">到账金额</span>
              <span style="margin:0px 0px 0px 15px;color:red;">{{(number-(number%100))/100}}.00元</span>
            </div>
            <van-field center  label="手续费" placeholder="免收手续费"  disabled :error="true"/>
        </van-cell-group>
      </section>
      <section>
            <!-- <van-cell :title="titledesc" style="font-size:10px;"/> -->
            <div style="margin:15px;color:#999">{{titledesc}}</div>
            <div style="text-align:center;">
                  <div style="margin:30px 15px 0px 15px;" @click="JumpCash">
                    <van-button type="default" size="large" style="background:#d81e06;color:#ffffff">申请提现</van-button>
                  </div>
                  <div style="font-size:16px;margin-top:15px;color:red;" @click="JumpDeti">查看提现记录<van-icon name="arrow" style="font-size:14px;" /></div>
            </div>
      </section>
      <van-dialog v-model="moneyshow" :show-confirm-button="false" title="我的收钱二维码" :close-on-click-overlay="true">
        <div style="text-align:center;"><img :src="wxMoneyQrcode" style="width:80%"/></div>
      </van-dialog>
  </section>
</template>
<script>
import notice from "../../assets/icon/icon_notices.png";
import { RadioGroup, Radio } from "vant";
export default {
  data() {
    return {
      id: 337466,
      url: "http://shg.yuf2.cn:8080/shg-api/api/",
      userdata: {},
      wxMoneyQrcode: "",
      alipay: "",
      ylhId: "",
      nitice: "满2000佣金币即可提现，需整百提现",
      notice_icon: notice,
      number: "",
      money: 999,
      titledesc:
        "每天可成功兑换一次，提现审核时间为9:00-21:00,审核成功后客服会根据您提供的微信二维码，进行打款操作",
      times: "",
      moneyshow: false,
      radio: "",
      type: "",
      ylhPhone: ""
    };
  },
  mounted() {
    // var dataJson = JSON.parse(
    //   decodeURIComponent(this.cookies.getCookie("userData"))
    // );
    // this.id = dataJson.id;
    this.getUserData();
  },
  methods: {
    onchange() {
      if (this.radio == "支付宝") {
        // alert("支付宝");
        this.type = "支付宝";
      } else if (this.radio == "微信") {
        this.type = "微信";
        // alert("微信");
      } else if (this.radio == "云联惠") {
        this.type = "云联惠";
        // alert("云联惠");
      }
    },
    JumpMoneyShowScan() {
      this.moneyshow = true;
    },
    getUserData() {
      // 缓存指针
      let _this = this;
      if (_this.id == "") {
        _this.$toast("当前您还未登录哦");
      } else {
        // 此处使用node做了代理
        var time = new Date();
        var times = Date.parse(time);
        _this.times = times;
        this.$axios
          .get(_this.url + "/integral/total?userId=" + _this.id)
          .then(function(response) {
            // 将得到的数据放到vue中的data
            _this.userdata = response.data.result;
            _this.money = _this.userdata.whiteIntegral;
            _this.ylhPhone = response.data.result.ylhPhone;
            _this.alipay = _this.userdata.alipay;
            _this.wxMoneyQrcode =
              _this.userdata.wxMoneyQrcode + "?time=" + times;
            //console.log(_this.userdata);
          })
          .catch(function(error) {
            console.log(error);
          });
      }
    },
    myFocus() {
      if (this.money < 1000) {
        this.$toast("您的佣金币还未到达1000哦");
        this.number = "";
      } else if (this.number > this.money) {
        this.$toast("您输入的佣金币超过了可提现佣金币的余额");
        this.number = "";
      }
    },
    JumpTotalMoney() {
      this.number = this.money - this.money % 100;
      if (this.money < 2000) {
        this.$toast("满2000佣金币才能提现哦");
        this.number = "";
      }
    },
    JumpBindingMoneysacn() {
      this.$router.push({
        path: "/shg",
        name: "bindingmoneyscan"
      });
    },
    JumpCash() {
      if (this.money < 2000) {
        this.$toast("满2000佣金币才能提现哦");
      } else if (this.number < 2000) {
        this.$toast("您输入的佣金币要大于或等于2000");
        this.number = "";
      } else {
        if (this.type == "支付宝") {
          this.tixianalipay();
        } else if (this.type == "微信") {
          this.tixianweixin();
        } else if (this.type == "云联惠") {
          this.tixianyunlianhui();
        }
      }
    },
    tixianalipay() {
      // 缓存指针
      let _this = this;
      // alert(_this.type + "1");
      if (_this.id == "") {
        _this.$toast("当前您还未登录哦");
      } else {
        // 此处使用node做了代理
        this.$axios
          .post(
            _this.url +
              "/integral/extract/alipay?userId=" +
              _this.id +
              "&integral=" +
              _this.number
          )
          .then(function(response) {
            // 将得到的数据放到vue中的data
            _this.$toast(response.data.message);
            _this.getUserData();
            // console.log(response);
          })
          .catch(function(error) {
            console.log(error);
          });
      }
    },
    tixianweixin() {
      // alert(this.type + "2");
      // 缓存指针
      let _this = this;
      if (_this.id == "") {
        _this.$toast("当前您还未登录哦");
      } else {
        // 此处使用node做了代理
        this.$axios
          .post(
            _this.url +
              "/integral/extract/wechat?userId=" +
              _this.id +
              "&integral=" +
              _this.number
          )
          .then(function(response) {
            // 将得到的数据放到vue中的data
            _this.$toast(response.data.message);
            _this.getUserData();
            // console.log(response);
          })
          .catch(function(error) {
            console.log(error);
          });
      }
    },
    tixianyunlianhui() {
      // alert(this.type + "3");
      // 缓存指针
      let _this = this;
      if (_this.id == "") {
        _this.$toast("当前您还未登录哦");
      } else {
        // 此处使用node做了代理
        this.$axios
          .post(
            _this.url +
              "/integral/submit_extract?userId=" +
              _this.id +
              "&integral=" +
              _this.number +
              "&ylhId=" +
              _this.ylhId +
              "&phone=" +
              _this.ylhPhone
          )
          .then(function(response) {
            // 将得到的数据放到vue中的data
            // console.log(response);
            _this.$toast(response.data.message);
            _this.getUserData();
          })
          .catch(function(error) {
            console.log(error);
          });
      }
    },
    JumpDeti() {
      this.$router.push({
        path: "/shg",
        name: "commissions",
        params: {
          data: 3
        }
      });
    }
  },
  watch: {
    // 监测路由变化,只要变化了就调用获取路由参数方法将数据存储本组件即可
    $route: "getParams"
  }
};
</script>
<style>
@import "../../common/css/fontface.css";
body {
  background: #f1f1f1;
}
</style>
