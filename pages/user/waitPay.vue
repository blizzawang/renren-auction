<template>
  <div style="width:500px;float:left">
    <div class="Jdbox_head">
      <img src="~@/assets/images/logo.png" alt="" /><span class="bank"
        >收银台</span
      >
      <!-- <ul>
        <li><span></span><span>退出</span></li>
        <li>我的订单</li>
        <li>支付帮助</li>
      </ul> -->
    </div>
    <div class="Jdbox_BuySuc">
      <dl>
        <dt><img src="~@/assets/images/waitPay/saoyisao.png" alt="" /></dt>
        <dd>
          <span>订单提交成功，请尽快付款！订单号：{{ order.orderNo }}</span>
          <span style="margin-right:100px"
            >应付金额<font>{{
              Number(order.shopPrice) + Number(order.shopFreight)
            }}</font
            >元</span
          >
        </dd>
        <dd>
          <span>推荐使用</span>
          <span
            >扫码支付请您在<font>24小时</font>内完成支付，否则订单会被自动取消(库存紧订单请参见详情页时限)</span
          >
          <span style="margin-right:100px">订单详细</span>
        </dd>
      </dl>
    </div>
    <div class="Jd_Con" style="width:950px">
      <p class="JdCon_title">
        <img src="~@/assets/images/waitPay/title.png" alt="" />
      </p>
      <div class="Jd_Fenqi">
        <ul>
          <li>
            <img src="~@/assets/images/waitPay/BAITIAO_2.0.png" alt="" />打白条
          </li>
          <li>
            <span>可用额度 7275.38元</span>
            <span>白条还款日 2018-01-27</span>
            <span><font>优惠</font>随机立减(最高10元)</span>
          </li>
          <li>
            支付<font>{{
              Number(order.shopPrice) + Number(order.shopFreight)
            }}</font
            >元
          </li>
        </ul>
        <ol>
          <li>
            <p>不分期</p>
            <p>0服务费</p>
          </li>
          <li style="cursor: pointer; border: 1px solid rgb(201, 223, 255);">
            <p>3期</p>
            <p>9.48元/期</p>
          </li>
          <li>
            <p>6期</p>
            <p>4.94元/期</p>
          </li>
          <li>
            <p>12期</p>
            <p>2.35元/期</p>
          </li>
          <li>
            <p>24期</p>
            <p>1.44元/期</p>
          </li>
        </ol>
      </div>
      <div class="Jd_main">
        <ul>
          <li>
            <span>
              <img
                src="~@/assets/images/waitPay/XJKCONSUME.png"
                alt=""
              />人人拍卖小金库
            </span>
            <span>未开通小金库</span>
          </li>
          <li>
            <span>
              <img src="~@/assets/images/waitPay/CMB.png" alt="" />招商银行
            </span>
            <span>信用卡(4337)</span>
            <span><font>优惠</font>单单减最高99元</span>
          </li>
          <li>
            <button style="cursor: pointer; color: rgb(103, 164, 255);">
              更多付款方式
            </button>
            <button style="cursor: pointer; color: rgb(103, 164, 255);">
              添加新卡/网银支付
            </button>
          </li>
          <li>
            <p>请输入6位数字支付密码</p>
            <input type="password" v-model="payInfo.password" /><span
              style="cursor: pointer; color: rgb(103, 164, 255);"
              >忘记支付密码？</span
            >
          </li>
          <li>
            <button
              @click="pay"
              style="cursor: pointer; background: rgb(252, 110, 108);"
            >
              立即支付
            </button>
          </li>
        </ul>
      </div>
    </div>
    <div class="Jd_footer">
      <ul>
        <li>
          <img src="~@/assets/images/waitPay/weixin.png" alt="" />微信支付
        </li>
        <li>
          <img
            src="~@/assets/images/waitPay/zhifubao.png"
            style="weight:auto;height:30px;"
            alt=""
            @click="toPayByZhifubao"
          />支付宝
        </li>
      </ul>
    </div>
    <!-- <div class="Jd_foots">
      <p>
        <span>Copyright @2004-2017 谷粒学院gulimall.com 版权所有</span>
        <span>
          <img src="~@/assets/images/waitPay/foots.png" alt="" />
        </span>
      </p>
    </div> -->
  </div>
</template>

<script>
import "~/assets/css/waitPay/style.css";
import cookie from "js-cookie";

export default {
  data() {
    return {
      order: {}, // 订单详情
      userId: 0,
      payInfo: {},
    };
  },
  mounted() {
    this.getOrderData();
  },
  methods: {
    // 点击支付宝按钮，跳转至支付宝页面进行支付
    toPayByZhifubao() {},
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
    // 获取上一页传递过来的订单信息
    getOrderData() {
      this.order = this.$route.query.order;
    },
    // 点击立即支付按钮，支付订单
    pay() {
      // 封装数据
      this.getCookieUserId();
      this.payInfo.userId = this.userId;
      this.payInfo.orderNo = this.order.orderNo;
      this.$axios.post("/order/pay", this.payInfo).then((response) => {
        let message = response.data.message;
        if (message === "支付成功") {
          // 密码正确，支付成功
          this.$message({
            message: message,
            type: "success",
          });
          // 跳转至购买记录页面
          this.$router.push("/user/buyRecord");
        } else {
          // 密码错误，支付失败
          this.$message({
            message: message,
            type: "error",
          });
        }
      });
    },
  },
};
</script>

<style></style>
