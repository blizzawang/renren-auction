<template>
  <div>
    <img-zoom
      style="margin-left:40px"
      :src="shopInfo.shopSmallImage"
      width="450"
      height="450"
      :bigsrc="shopInfo.shopBigImage"
      :configs="configs"
    ></img-zoom>
    <div style="border:1px red solid;width:45%;float:right;margin-right:210px">
      <div
        v-if="this.shopInfo.status === -1"
        style="width:220px;height:70px;border:4px #980000 solid;float:left;
          text-align:center;line-height:70px;color:#980000;font-size:30px;
          margin-top:12%;margin-left:12%;position:absolute;
          transform: rotate(13deg);-o-transform:rotate(13deg);    
          -webkit-transform:rotate(13deg);-moz-transform:rotate(13deg);"
      >
        已撤拍
      </div>
      <div
        v-if="this.shopInfo.status === 2"
        style="width:220px;height:70px;border:4px #980000 solid;float:left;
          text-align:center;line-height:70px;color:#980000;font-size:30px;
          margin-top:12%;margin-left:12%;position:absolute;
          transform: rotate(13deg);-o-transform:rotate(13deg);    
          -webkit-transform:rotate(13deg);-moz-transform:rotate(13deg);"
      >
        已成交
      </div>
      <p
        style="font-weight:1000;padding-top:10px;line-height:28px;margin-bottom:5px"
      >
        <font face="microsoft yahei" size="4px" color="#666">
          {{ shopInfo.shopName }}
        </font>
      </p>
      <div style="border:1px blue solid;width:100%;margin-top:20px">
        <span
          style="color:#888;font-size:17px;font-weight:blod;margin-left:10px"
          >距结束</span
        ><span style="margin-left:50px"
          ><span style="color:#980000;font-size:25px">{{ shopInfo.day }}</span>
          天
          <span style="color:#980000;font-size:25px">{{ shopInfo.hour }}</span>
          时
          <span style="color:#980000;font-size:25px">{{
            shopInfo.minute
          }}</span>
          分
          <span style="color:#980000;font-size:25px">{{
            shopInfo.second
          }}</span>
          秒</span
        >
      </div>
      <div
        style="border:1px black solid;width:100%;height:126px;margin-top:20px;
        background-color:#E6E6E6;padding-top:15px"
      >
        <span style="font-size:17px;margin-left:10px">当前价</span>
        <span style="color:#c00;font-size:19px;margin-left:70px"
          >¥{{ shopInfo.shopCurrentPrice }}</span
        >
        <br />
        <br />
        <span style="font-size:17px;margin-left:10px">保证金</span>
        <span style="color:#c00;font-size:19px;margin-left:70px"
          >¥{{ shopInfo.earnestMoney }}</span
        >
        <br />
        <br />
        <span style="font-size:17px;margin-left:10px">领先者</span>
        <span style="color:#c00;font-size:19px;margin-left:70px">{{
          userInfo.nickName
        }}</span>
      </div>
      <div style="border:1px green solid;width:100%;text-align:center">
        <p style="font-size:17px;margin-top:10px">竞拍出价</p>
        <button style="width:38px;height:38px" @click="subtract">
          <i style="font-size:20px">-</i>
        </button>
        <input
          style="width:510px;height:38px;text-align:center;font-size:20px;display:inline-block"
          :value="offerPrice"
        />
        <button style="width:38px;height:38px" @click="add">
          <i style="font-size:20px">+</i>
        </button>
      </div>
      <div
        style="border:1px yellow solid;width:100%;height:100px;text-align:center;
        margin-top:15px"
      >
        <el-button type="danger" style="width:290px" @click="auction"
          >竞拍出价</el-button
        >
        <el-button type="primary" style="width:290px">代理出价</el-button>
      </div>
    </div>
  </div>
</template>

<script>
import cookie from "js-cookie";
import imgZoom from "vue2.0-zoom";
import "~/assets/shop_detail_css/shop.css";
import "~/assets/shop_detail_css/shop.css";

export default {
  components: { imgZoom },
  data() {
    return {
      userInfo: {}, // 出价最高用户
      offerRecord: {}, // 出价记录
      userId: 0, // 用户id
      shopInfo: {
        shopSmallImage: "",
        shopBigImage: "",
      },
      offerPrice: 0, // 买家出价
      shopId: 0, // 商品id
      configs: {
        width: 650,
        height: 350,
        maskWidth: 100,
        maskHeight: 100,
        maskColor: "red",
        maskOpacity: 0.2,
      },
    };
  },
  mounted() {
    // 通过商品id获取到商品的详细信息
    this.getShopDetailById();
    // 获取领先者信息
    this.getFirstPriceUser();
  },
  methods: {
    // 定时器，倒计时拍卖时间
    startTimer() {
      let myVue = this; // 得到Vue实例
      if (
        this.shopInfo.day >= 0 &&
        this.shopInfo.hour >= 0 &&
        this.shopInfo.minute >= 0 &&
        this.shopInfo.second >= 0
      ) {
        let timer = setInterval(function() {
          // 当天、时、分、秒均为0时，停止定时器（为了防止越界，判断是否等于1即可）
          if (
            myVue.shopInfo.day === 1 &&
            myVue.shopInfo.hour === 1 &&
            myVue.shopInfo.minute === 1 &&
            myVue.shopInfo.second === 1
          ) {
            clearInterval(timer);
          }

          // 当秒数减到0时，就让分钟减1
          if (myVue.shopInfo.second === 0) {
            // 若分钟不为0，则让分钟减1，并重置秒数
            if (myVue.shopInfo.minute !== 0) {
              myVue.shopInfo.minute = myVue.shopInfo.minute - 1;
              // 重置秒数
              myVue.shopInfo.second = 60;
            } else {
              // 若分钟为0，则查看小时和天数是否为0
              if (myVue.shopInfo.hour === 0 && myVue.shopInfo.day === 0) {
                // 若为0，则拍品时间结束
                // clearInterval(timer);
              } else {
                // 只要其中一个不为0，则重置秒数
                myVue.shopInfo.second = 60;
              }
            }
          }
          // 当分钟减到0时，就让小时减1
          if (myVue.shopInfo.minute === 0) {
            // 若小时不为0，则让小时减1，并重置分钟
            if (myVue.shopInfo.hour !== 0) {
              myVue.shopInfo.hour = myVue.shopInfo.hour - 1;
              // 重置分钟
              myVue.shopInfo.minute = 60;
            } else {
              // 若小时为0，则查看天数是否为0
              if (myVue.shopInfo.day === 0) {
                // 若为0，则拍品时间结束
                // clearInterval(timer);
              } else {
                // 若不为0，则重置分钟
                myVue.shopInfo.minute = 60;
              }
            }
          }
          // 当小时减到0时，就让天数减1
          if (myVue.shopInfo.hour === 0) {
            // 若天数不为0，则让天数减1，并重置小时
            if (myVue.shopInfo.day !== 0) {
              myVue.shopInfo.day = myVue.shopInfo.day - 1;
              // 重置小时
              myVue.shopInfo.hour = 24;
            } else {
              // 若天数为0，则拍品时间结束
              // clearInterval(timer);
            }
          }
          // 每隔一秒，就让当前秒数减1
          myVue.shopInfo.second = myVue.shopInfo.second - 1;
        }, 1000);
      } else {
        // 若有负数，则说明拍品已经结束拍卖，将所有时间置为0
        myVue.shopInfo.day = 0;
        myVue.shopInfo.hour = 0;
        myVue.shopInfo.minute = 0;
        myVue.shopInfo.second = 0;
        // 提示用户当前拍品已结束拍卖
        console.log("当前拍品已结束拍卖...");
      }
    },
    // 获取Cookie中当前用户的id
    getCookieUserId() {
      // 判断当前Cookie中是否包含用户信息
      let userInfo = cookie.get("userInfo");
      if (!userInfo) {
        this.userInfo = null;
        return;
      }
      userInfo = JSON.parse(userInfo);
      this.userId = userInfo.id;
    },
    // 获取领先者信息
    getFirstPriceUser() {
      this.$axios
        .$get(`/login/offerRecord/first/${this.shopId}`)
        .then((response) => {
          this.userInfo = response.data.userInfo;
        });
    },
    // 通过商品id获取到商品的详细信息
    getShopDetailById() {
      this.shopId = this.$route.query.id;
      this.$axios.$get(`/login/shop/detail/${this.shopId}`).then((response) => {
        this.shopInfo = response.data.shopInfo;
        // 设置初始竞价
        this.offerPrice = this.shopInfo.shopCurrentPrice;
        // 启动定时器，倒计时拍卖时间
        this.startTimer();
      });
    },
    add() {
      this.offerPrice = this.offerPrice + this.shopInfo.shopAddPrice;
    },
    subtract() {
      const h = this.$createElement;
      // 竞拍出价不得低于商品当前价格
      let tempPrice = this.offerPrice - this.shopInfo.shopAddPrice;
      if (tempPrice < this.shopInfo.shopCurrentPrice) {
        this.$notify({
          title: "温馨提示",
          type: "warning",
          offset: 100,
          message: h(
            "i",
            { style: "color: teal" },
            "竞拍出价不得低于拍品当前价格"
          ),
        });
      } else {
        this.offerPrice = this.offerPrice - this.shopInfo.shopAddPrice;
      }
    },
    // 参与竞拍
    auction() {
      if (this.shopInfo.status === 1) {
        let myVue = this;
        this.getCookieUserId();
        this.$confirm(
          "参与竞拍需缴纳拍品保证金¥100.00（冻结账户金额¥100.00）！是否继续?",
          "提示",
          {
            confirmButtonText: "确定",
            cancelButtonText: "取消",
            type: "warning",
          }
        )
          .then(() => {
            // 若商品状态为拍卖中
            if (myVue.shopInfo.status === 1) {
              this.$message({
                type: "success",
                message: "出价成功",
              });
              // 封装数据
              this.offerRecord.shopId = this.shopId;
              this.offerRecord.offerPrice = this.offerPrice;
              this.offerRecord.userId = this.userId;
              this.offerRecord.earnestPrice = this.shopInfo.earnestMoney;
              console.log(this.offerRecord);
              // 发送请求进行出价
              this.$axios
                .$post("/login/offerRecord/offer", this.offerRecord)
                .then((response) => {});
            } else if (myVue.shopInfo.status === 2) {
              console.log("该商品已成交");
            } else if (myVue.shopInfo.status === -1) {
              console.log("该商品已撤拍");
            }
          })
          .catch(() => {
            this.$message({
              type: "info",
              message: "取消出价",
            });
          });
      } else if (this.shopInfo.status === 2) {
        this.$message({
          message: "该商品已成交",
          type: "warning",
        });
      } else if (this.shopInfo.status === -1) {
        this.$message({
          message: "该商品已撤拍",
          type: "warning",
        });
      }
    },
  },
};
</script>

<style></style>
