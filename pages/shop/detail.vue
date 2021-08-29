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
        })
        .catch(() => {
          this.$message({
            type: "info",
            message: "取消出价",
          });
        });
    },
  },
};
</script>

<style></style>
