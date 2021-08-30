<template>
  <div>
    <div class="section" style="margin-left:250px">
      <!--收货人信息-->
      <div class="top-2">
        <span>收货人信息</span>
        <span style="margin-right:100px">新增收货地址</span>
      </div>

      <!--地址-->
      <div class="top-3">
        <p>{{ order.buyerName }}</p>
        <span>{{ order.buyerAddress }} {{ order.phone }}</span>
      </div>
      <p class="p2">更多地址︾</p>
      <div class="hh1"></div>
      <!--********************************************************************************************-->
      <div class="top-4">
        <p>人人拍卖</p>
        <p>省运费·无续重·随时取</p>
        <p class="xiang">详情</p>
      </div>

      <!--地址-->
      <div class="top-5" style="width:920px">
        <p>江西省吉安市</p>
        <span>江西省 吉安市 青原区 井冈山大学 XXX 186****8729</span
        ><img src="~@/assets/images/shopSettlement/09.png" /><span
          >距收货人500米</span
        >
        <p class="p3">距离最近</p>
        <span class="xiu">修改自提点</span>
      </div>
      <div class="hh1"></div>

      <!--支付方式-->
      <h4 class="h4">支付方式</h4>

      <div class="top-6">
        <p>货到付款</p>
        <p><span>惠</span>在线支付</p>
      </div>
      <div class="hh1"></div>
      <!--送货清单-->
      <h4 class="h4" style="margin-top: 5px;">送货清单</h4>
      <div class="top_1">
        <div class="to_left">
          <h5>
            <span class="peisong">配送方式</span
            ><span class="dui"
              ><img src="~@/assets/images/shopSettlement/i_03.png" />
              对应商品</span
            >
          </h5>
          <div class="box">
            顺丰快递
          </div>
          <p class="biao">
            <span class="til">标 准 达 ：</span>
            <span class="con">预计 12月16日[今天] 15:00-19:00 送达</span>
            <a href="#">修改</a>
          </p>
          <div class="updata-1">
            <img src="~@/assets/images/shopSettlement/im_06.png" />
            <span>拍立达 标准达</span>
            <span style="color: black;"> 配送服务全面升级</span>
          </div>
          <div class="hh1"></div>
          <p class="tui">
            <span class="til">退换无忧：</span>
            <span class="con">
              <input type="checkbox" />
              自签收后7天内退货，15天内换<span
                style="font-size: 12px;margin-left: 5px;"
              >
                ￥ 0.50</span
              ><br />
              <span class="nul">货，</span>可享1次上门取件服务 ﹀
            </span>
          </p>
          <div class="updata-2">
            <img src="~@/assets/images/shopSettlement/im_11.png" />
            <span>拍立达运费大促（限自营中小件）</span>
          </div>

          <p></p>
          <p class="kg" style="color:#666666;margin-top: 13px;font-size: 12px">
            总重量 :<span style="color:#999999;font-size: 12px">0.095kg</span>
          </p>
        </div>
        <div class="to_right" style="width:500px">
          <h5>商家：{{ order.storeName }}自营</h5>
          <div>
            <button>换购</button
            ><span>已购满20.00元，再加49.90元，可返回购物车领取赠品</span>
          </div>
          <!--图片-->
          <div class="yun1">
            <img :src="order.shopIndexImage" class="yun" />
            <div class="mi">
              <p>
                {{ order.shopName }}
                <span style="color: red;"> ￥ {{ order.shopPrice }}</span>
                <span> x1 </span>
                <span>有货</span>
              </p>
              <p>商品描述:...... <span>0.095kg</span></p>
              <p class="tui-1">
                <img
                  src="~@/assets/images/shopSettlement/i_07.png"
                />支持7天无理由退货
              </p>
            </div>
          </div>

          <div class="hh1"></div>
          <p>
            退换无忧
            <span class="money" style="float:left">￥ 0.00</span>
          </p>
        </div>
      </div>
      <div class="bto">
        <div class="hh2"></div>
        <h4 class="float" style="width:500px">发票信息</h4>
        <div class="note float">
          <img src="~@/assets/images/shopSettlement/i_11.png" />
          <span>开企业抬头发票须填写纳税人识别号，以免影响报销</span>
        </div>
        <ul style="clear: both;">
          <li>
            电子普通发票 <img src="~@/assets/images/shopSettlement/i_14.png" />
          </li>
          <li>个人</li>
          <li>商品明细</li>
          <li>
            <a href="">修改</a>
          </li>
        </ul>
        <div class="hh3"></div>
        <h4 class="clear">使用优惠/礼品卡/抵用 ^</h4>
        <ul>
          <li class="red">优惠卡</li>
          <li>礼品卡</li>
          <li>京豆</li>
          <li>余额</li>
          <li>领奖码</li>
        </ul>
        <div class="tuijian clear">
          <input type="checkbox" />
          <span>优惠组合推荐</span>
        </div>
      </div>
      <div class="xia">
        <div class="qian">
          <p class="qian_y" style="margin-right:100px">
            <span>1</span>
            <span>件商品，总商品金额：</span>
            <span class="rmb">￥{{ order.shopPrice }}</span>
          </p>
          <p class="qian_y" style="margin-right:100px">
            <span>返现：</span>
            <span class="rmb"> -￥0.00</span>
          </p>
          <p class="qian_y" style="margin-right:100px">
            <span>运费： </span>
            <span class="rmb"> &nbsp; ￥{{ order.shopFreight }}</span>
          </p>
          <p class="qian_y">
            <span>服务费： </span>
            <span class="rmb" style="margin-right:100px"> &nbsp; ￥0.00</span>
          </p>
          <p class="qian_y">
            <span>退换无忧： </span>
            <span class="rmb" style="margin-right:100px"> &nbsp; ￥0.00</span>
          </p>
        </div>

        <div class="yfze">
          <p class="yfze_a">
            <span class="z">应付总额：</span
            ><span class="hq" style="margin-right:100px"
              >￥{{ Number(order.shopPrice) + Number(order.shopFreight) }}</span
            >
          </p>
          <p class="yfze_b" style="margin-right:100px">
            寄送至： {{ order.buyerAddress }} 收货人：{{ order.buyerName }}
            {{ order.phone }}
          </p>
        </div>
        <button
          style="background-color:#E2383B;float:right;margin-right:100px;width:100px;height:40px;font-size:20px;padding:5px;border-radius:5px;color:white;text-align:center;border:none;"
        >
          提交订单
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import "~/assets/css/shopSettlement/JD2.css";

export default {
  data() {
    return {
      order: {}, // 订单信息
    };
  },
  mounted() {
    // 获取结算信息
    this.getShopSettlement();
  },
  methods: {
    getShopSettlement() {
      this.$axios.get(`/order/get/${this.$route.query.id}`).then((response) => {
        this.order = response.data.data.order;
      });
    },
  },
};
</script>

<style></style>
