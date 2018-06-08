<template>
  <section style="">
    <div style="position:absolute;background:#ffffff">
        <div style="position:fixed;z-index:999;width:100%">
          <van-cell-group>
            <van-search v-model="keyWord" show-action @search="Search"  placeholder="搜索您要搜索的宝贝">
              <van-button slot="action" @click="Search" type="danger" size="small">搜索</van-button>
            </van-search>
          </van-cell-group>
        </div> 
    </div>
    <section style="height:50px;"></section>
    <section>
      <van-tabs v-model="active">
        <van-tab>
        <div slot="title" @click="superSearch">
          综合排序
        </div>
        <van-list>
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
                  <div v-for="(r, key) in articles" :key="key" @click="JumpPageDetails(r.itemid,r.qfee,r.itempic,r.fl,r.itemfee2,r.itemfee,r.itemmsell,r.itemyhj_url,r.itemyhj_yl,r.itemyhj_etime,r.actid,r.maijia_id)">
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
        </van-list>
        </van-tab>

        <van-tab>
        <div slot="title" @click="onlyqSearch">
          仅看有券
        </div>
        <van-list>
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
                  <div v-for="(r, key) in onlyqdata" :key="key" @click="JumpPageDetails(r.itemid,r.qfee,r.itempic,r.fl,r.itemfee2,r.itemfee,r.itemmsell,r.itemyhj_url,r.itemyhj_yl,r.itemyhj_etime,r.actid,r.maijia_id)">
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
        </van-list>
        </van-tab>

        <van-tab>
        <div slot="title" @click="ishtSearch">
          海外商品
        </div>
        <van-list>
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
                  <div v-for="(r, key) in ishtdata" :key="key" @click="JumpPageDetails(r.itemid,r.qfee,r.itempic,r.fl,r.itemfee2,r.itemfee,r.itemmsell,r.itemyhj_url,r.itemyhj_yl,r.itemyhj_etime,r.actid,r.maijia_id)">
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
        </van-list>
        </van-tab>

        <van-tab>
        <div slot="title" @click="issiteSearch">
          站内精选
        </div>
        <van-list>
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
                  <div v-for="(r, key) in issitedata" :key="key" @click="JumpPageDetails(r.itemid,r.qfee,r.itempic,r.fl,r.itemfee2,r.itemfee,r.itemmsell,r.itemyhj_url,r.itemyhj_yl,r.itemyhj_etime,r.actid,r.maijia_id)">
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
        </van-list>
        </van-tab>

        <van-tab>
        <div slot="title" @click="tianmaoSearch">
          天猫优先
        </div>
        <van-list>
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
                  <div v-for="(r, key) in tianmaodata" :key="key" @click="JumpPageDetails(r.itemid,r.qfee,r.itempic,r.fl,r.itemfee2,r.itemfee,r.itemmsell,r.itemyhj_url,r.itemyhj_yl,r.itemyhj_etime,r.actid,r.maijia_id)">
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
        </van-list>
        </van-tab>

        <van-tab>
        <div slot="title" @click="daysaleSearch">
          30日销量
        </div>
        <van-list>
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
                  <div v-for="(r, key) in daysaledata" :key="key" @click="JumpPageDetails(r.itemid,r.qfee,r.itempic,r.fl,r.itemfee2,r.itemfee,r.itemmsell,r.itemyhj_url,r.itemyhj_yl,r.itemyhj_etime,r.actid,r.maijia_id)">
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
        </van-list>
        </van-tab>

        <van-tab>
        <div slot="title" @click="quanhoujiaSearch">
          券后价 
          <van-icon v-if="number%2==0" name="e65e" style="font-size:14px;"/>
          <van-icon v-else name="e65d" style="font-size:14px;"/>
        </div>
        <van-list>
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
                  <div v-for="(r, key) in quanhoujiadata" :key="key" @click="JumpPageDetails(r.itemid,r.qfee,r.itempic,r.fl,r.itemfee2,r.itemfee,r.itemmsell,r.itemyhj_url,r.itemyhj_yl,r.itemyhj_etime,r.actid,r.maijia_id)">
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
        </van-list>
        </van-tab>

      </van-tabs>
    </section>
    <section>
      
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
import icon_up from "../../assets/icon/icon_up.png";
const API_PROXY = "https://bird.ioliu.cn/v1/?url=";
// _this.url2 +
//             "&q=" +
//             _this.keyWord +
//             "&limit=" +
//             _this.limit +
//             "&size=" +
//             _this.pagesize +
//             "&onlyq=" +
//             _this.onlyq +
//             "&isht=" +
//             _this.isht +
//             "&issite=" +
//             _this.issite +
//             "&shopid=&actid=&nbnice=1&openid="
export default {
  data() {
    return {
      url: "http://shg.yuf2.cn:8080/shg-api/api/",
      url2:
        "http://yuf2.cn/app/index.php?c=entry&i=2&m=bsht_tbkquan&do=gethdinfo&itemid=&sid=&itemfee=&itempic=&nb_shopid=",
      keyWord: "",
      rowlength: "",
      isLoading: true,
      articles: {},
      onlyqdata: {},
      ishtdata: {},
      issitedata: {},
      tianmaodata: {},
      daysaledata: {},
      quanhoujiadata: {},
      messages: "",
      currentName: "综合排序",
      active: 0,
      pagesize: 20,
      onlyq: 1, //1只看有券，0全部
      isht: 1, //1海外商品 0 全部
      issite: 1, //1 站内精选 0 全部
      number: 0
    };
  },
  mounted() {
    this.superSearch();
  },
  methods: {
    Search() {
      this.active = 0;
      this.superSearch();
    },
    superSearch() {
      let _this = this;
      _this.currentName = "综合排序";
      let sw = true;
      let limit = 1;
      // 此处使用node做了代理
      this.$axios
        .post(
          _this.url2 +
            "&q=" +
            _this.keyWord +
            "&limit=" +
            limit +
            "&size=" +
            _this.pagesize +
            "&shopid=&actid=&nbnice=1&openid="
        )
        .then(function(response) {
          // 将得到的数据放到vue中的data
          _this.articles = response.data.nb_qlist;
          var lengths = response.data.nb_qlist.length;
          _this.rowlength = lengths;
          limit += 1;
          // console.log(_this.articles);
        })
        .catch(function(error) {
          console.log(error);
        });
      window.addEventListener("scroll", function() {
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
        // if (_this.currentName == "综合排序") {
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
                  limit++ +
                  "&size=" +
                  _this.pagesize +
                  "&shopid=&actid=&nbnice=1&openid="
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
        // }
      });
    },
    onlyqSearch() {
      let _this = this;
      _this.currentName = "仅看有券";
      let sw = true;
      let limit = 1;
      // 此处使用node做了代理
      this.$axios
        .post(
          _this.url2 +
            "&q=" +
            _this.keyWord +
            "&limit=" +
            limit +
            "&size=" +
            _this.pagesize +
            "&onlyq=" +
            _this.onlyq +
            "&shopid=&actid=&nbnice=1&openid="
        )
        .then(function(response) {
          // 将得到的数据放到vue中的data
          _this.onlyqdata = response.data.nb_qlist;
          var lengths = response.data.nb_qlist.length;
          _this.rowlength = lengths;
          limit += 1;
        })
        .catch(function(error) {
          console.log(error);
        });
      window.addEventListener("scroll", function() {
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
        // if ((_this.currentName = "仅看有券")) {
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
                  limit++ +
                  "&size=" +
                  _this.pagesize +
                  "&onlyq=" +
                  _this.onlyq +
                  "&shopid=&actid=&nbnice=1&openid="
              )
              .then(function(response) {
                // 将新获取的数据push到vue中的data，就会反应到视图中了
                var lengths = response.data.nb_qlist.length;
                for (var i = 0; i < lengths; i++) {
                  _this.onlyqdata.push(response.data.nb_qlist[i]);
                }
                // console.log(lengths)
                // 数据更新完毕，将开关打开
                sw = true;
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
        // }
      });
    },
    ishtSearch() {
      let _this = this;
      _this.currentName = "海外商品";
      let sw = true;
      let limit = 1;
      // 此处使用node做了代理
      this.$axios
        .post(
          _this.url2 +
            "&q=" +
            _this.keyWord +
            "&limit=" +
            limit +
            "&size=" +
            _this.pagesize +
            "&isht=" +
            _this.isht +
            "&shopid=&actid=&nbnice=1&openid="
        )
        .then(function(response) {
          // 将得到的数据放到vue中的data
          _this.ishtdata = response.data.nb_qlist;
          var lengths = response.data.nb_qlist.length;
          _this.rowlength = lengths;
          limit += 1;
        })
        .catch(function(error) {
          console.log(error);
        });
      window.addEventListener("scroll", function() {
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
        // if (_this.currentName == "海外商品") {
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
                  limit++ +
                  "&size=" +
                  _this.pagesize +
                  "&isht=" +
                  _this.isht +
                  "&shopid=&actid=&nbnice=1&openid="
              )
              .then(function(response) {
                // 将新获取的数据push到vue中的data，就会反应到视图中了
                var lengths = response.data.nb_qlist.length;
                for (var i = 0; i < lengths; i++) {
                  _this.ishtdata.push(response.data.nb_qlist[i]);
                }
                // console.log(lengths)
                // 数据更新完毕，将开关打开
                sw = true;
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
        // }
      });
    },
    issiteSearch() {
      let _this = this;
      _this.currentName == "站内精选";
      // alert(_this.currentName);
      let sw = true;
      let limit = 1;
      // 此处使用node做了代理
      this.$axios
        .post(
          _this.url2 +
            "&q=" +
            _this.keyWord +
            "&limit=" +
            limit +
            "&size=" +
            _this.pagesize +
            "&issite=" +
            _this.issite +
            "&shopid=&actid=&nbnice=1&openid="
        )
        .then(function(response) {
          // 将得到的数据放到vue中的data
          _this.issitedata = response.data.nb_qlist;
          var lengths = response.data.nb_qlist.length;
          _this.rowlength = lengths;
          limit += 1;
        })
        .catch(function(error) {
          console.log(error);
        });
      window.addEventListener("scroll", function() {
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
        // if (_this.currentName == "站内精选") {
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
                  limit++ +
                  "&size=" +
                  _this.pagesize +
                  "&issite=" +
                  _this.issite +
                  "&shopid=&actid=&nbnice=1&openid="
              )
              .then(function(response) {
                // 将新获取的数据push到vue中的data，就会反应到视图中了
                var lengths = response.data.nb_qlist.length;
                for (var i = 0; i < lengths; i++) {
                  _this.issitedata.push(response.data.nb_qlist[i]);
                }
                // console.log(lengths)
                // 数据更新完毕，将开关打开
                sw = true;
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
        // }
      });
    },
    tianmaoSearch() {
      let _this = this;
      _this.currentName == "天猫优先";
      // alert(_this.currentName);
      let sw = true;
      let limit = 1;
      let type = 5;
      // 此处使用node做了代理
      this.$axios
        .post(
          _this.url2 +
            "&q=" +
            _this.keyWord +
            "&limit=" +
            limit +
            "&size=" +
            _this.pagesize +
            "&type=" +
            type +
            "&shopid=&actid=&nbnice=1&openid="
        )
        .then(function(response) {
          // 将得到的数据放到vue中的data
          _this.tianmaodata = response.data.nb_qlist;
          var lengths = response.data.nb_qlist.length;
          _this.rowlength = lengths;
          limit += 1;
          console.log(lengths);
        })
        .catch(function(error) {
          console.log(error);
        });
      window.addEventListener("scroll", function() {
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
        // if (_this.currentName == "天猫优先") {
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
                  limit++ +
                  "&size=" +
                  _this.pagesize +
                  "&type=" +
                  type +
                  "&shopid=&actid=&nbnice=1&openid="
              )
              .then(function(response) {
                // 将新获取的数据push到vue中的data，就会反应到视图中了
                var lengths = response.data.nb_qlist.length;
                for (var i = 0; i < lengths; i++) {
                  _this.tianmaodata.push(response.data.nb_qlist[i]);
                }
                console.log(lengths);
                // 数据更新完毕，将开关打开
                sw = true;
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
        // }
      });
    },
    daysaleSearch() {
      let _this = this;
      _this.currentName == "30日销量";
      // alert(_this.currentName);
      let sw = true;
      let limit = 1;
      let type = 3;
      // 此处使用node做了代理
      this.$axios
        .post(
          _this.url2 +
            "&q=" +
            _this.keyWord +
            "&limit=" +
            limit +
            "&size=" +
            _this.pagesize +
            "&type=" +
            type +
            "&shopid=&actid=&nbnice=1&openid="
        )
        .then(function(response) {
          // 将得到的数据放到vue中的data
          _this.daysaledata = response.data.nb_qlist;
          var lengths = response.data.nb_qlist.length;
          _this.rowlength = lengths;
          limit += 1;
          console.log(lengths);
        })
        .catch(function(error) {
          console.log(error);
        });
      window.addEventListener("scroll", function() {
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
        // if (_this.currentName == "30日销量") {
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
                  limit++ +
                  "&size=" +
                  _this.pagesize +
                  "&type=" +
                  type +
                  "&shopid=&actid=&nbnice=1&openid="
              )
              .then(function(response) {
                // 将新获取的数据push到vue中的data，就会反应到视图中了
                var lengths = response.data.nb_qlist.length;
                for (var i = 0; i < lengths; i++) {
                  _this.daysaledata.push(response.data.nb_qlist[i]);
                }
                console.log(lengths);
                // 数据更新完毕，将开关打开
                sw = true;
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
        // }
      });
    },
    quanhoujiaSearch() {
      let _this = this;
      _this.currentName == "券后价";
      let type = 4;
      _this.number++;
      if (_this.number % 2 == 0) {
        // alert("1");
        let sw = true;
        let limit = 1;
        this.$axios
          .post(
            _this.url2 +
              "&q=" +
              _this.keyWord +
              "&limit=" +
              limit +
              "&size=" +
              _this.pagesize +
              "&type=" +
              type +
              "&jiage=up" +
              "&shopid=&actid=&nbnice=1&openid="
          )
          .then(function(response) {
            // 将得到的数据放到vue中的data
            _this.quanhoujiadata = response.data.nb_qlist;
            var lengths = response.data.nb_qlist.length;
            _this.rowlength = lengths;
            limit += 1;
            // console.log(lengths);
          })
          .catch(function(error) {
            console.log(error);
          });
        window.addEventListener("scroll", function() {
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
          // if (_this.currentName == "券后价") {
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
                    limit++ +
                    "&size=" +
                    _this.pagesize +
                    "&type=" +
                    type +
                    "&jiage=up" +
                    "&shopid=&actid=&nbnice=1&openid="
                )
                .then(function(response) {
                  // 将新获取的数据push到vue中的data，就会反应到视图中了
                  var lengths = response.data.nb_qlist.length;
                  for (var i = 0; i < lengths; i++) {
                    _this.quanhoujiadata.push(response.data.nb_qlist[i]);
                  }
                  // console.log(lengths);
                  // 数据更新完毕，将开关打开
                  sw = true;
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
          // }
        });
      } else {
        // alert(2);
        let sw = true;
        let limit = 1;
        this.$axios
          .post(
            _this.url2 +
              "&q=" +
              _this.keyWord +
              "&limit=" +
              limit +
              "&size=" +
              _this.pagesize +
              "&type=" +
              type +
              "&jiage=down" +
              "&shopid=&actid=&nbnice=1&openid="
          )
          .then(function(response) {
            // 将得到的数据放到vue中的data
            _this.quanhoujiadata = response.data.nb_qlist;
            var lengths = response.data.nb_qlist.length;
            _this.rowlength = lengths;
            limit += 1;
            console.log(lengths);
          })
          .catch(function(error) {
            console.log(error);
          });
        window.addEventListener("scroll", function() {
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
          // if (_this.currentName == "券后价") {
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
                    limit++ +
                    "&size=" +
                    _this.pagesize +
                    "&type=" +
                    type +
                    "&jiage=down" +
                    "&shopid=&actid=&nbnice=1&openid="
                )
                .then(function(response) {
                  // 将新获取的数据push到vue中的data，就会反应到视图中了
                  var lengths = response.data.nb_qlist.length;
                  for (var i = 0; i < lengths; i++) {
                    _this.quanhoujiadata.push(response.data.nb_qlist[i]);
                  }
                  // console.log(lengths);
                  // 数据更新完毕，将开关打开
                  sw = true;
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
          // }
        });
      }
    },
    // 跳转商品详情页
    // JumpPageDetails(r.itemid,r.qfee,r.itempic,r.fl,r.itemfee2,r.itemfee,r.itemmsell,r.itemyhj_url,r.itemyhj_yl,r.itemyhj_etime,r.actid,r.maijia_id)
    JumpPageDetails(
      itemid,
      qfee,
      itempic,
      fl,
      itemfee2,
      itemfee,
      itemmsell,
      itemyhj_url,
      itemyhj_yl,
      itemyhj_etime,
      actid,
      maijia_id
    ) {
      if (actid == "" || actid == null) {
        actid = "actid";
      }
      this.$router.push({
        path: "/shg",
        name: "superPageDetails",
        query: {
          goodsId: itemid,
          qfee: qfee,
          itempic: itempic,
          fl: fl,
          itemfee2: itemfee2,
          itemfee: itemfee,
          itemmsell: itemmsell,
          itemyhj_url: itemyhj_url,
          itemyhj_yl: itemyhj_yl,
          itemyhj_etime: itemyhj_etime,
          actid: actid,
          maijia_id: maijia_id
        }
      });
    },
    JumpLove() {
      this.$router.push({
        path: "/shg",
        name: "love"
      });
    },
    JumpVip() {
      this.$router.push({
        path: "/shg",
        name: "vip"
      });
    },
    JumpIndex() {
      this.$router.push({
        path: "/shg",
        name: "indexs"
      });
    },
    JumpUser() {
      this.$router.push({
        path: "/shg",
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
@import "../../common/css/fontface.css";
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
