<template>
  <div class="main">
    <div class="block">
      <span class="demonstration"></span>
      <el-carousel height="340" autoplay>
        <el-carousel-item v-for="imgUrl in imgUrls" :key="imgUrl">
          <!-- <h3 class="small">{{ imgUrl }}</h3> -->
          <img :src="imgUrl" title="" />
        </el-carousel-item>
        <div class="card">
          <span>本站头条</span>
          <marquee direction="up" behavior="slide" scrolldelay="400">
            <p style="float:left;margin-left:10px;font-size:15px">
              <strong>[公告]暑假快乐！</strong>
              <br />
              <strong>[公告]这是一段测试文本</strong>
            </p></marquee
          >
          <div
            style="width:300px;height:100px;border:1px red solid;float:left;margin-top:10px"
          >
            <img
              src="~/assets/images/head.png"
              title=""
              style="width:50px;height:50px;float:left;margin:5px"
            />
            <p style="margin:5px;float:left;line-height:50px;font-size:15px">
              Hi,游客4564165498751
            </p>
            <button
              style="float:left;width:100px;height:30px;
              color: #fff;background-color: #e6a23c;
              border-color: #e6a23c;margin-left:15px;margin-top:5px;"
            >
              登录
            </button>
            <button
              style="float:left;width:100px;height:30px;
              color: #fff;background-color: #e6a23c;
              border-color: #e6a23c;margin-left:35px;margin-top:5px"
            >
              注册
            </button>
          </div>
          <div
            style="border:3px black solid;width:300px;height:75px;float:left"
          >
            <p
              style="float:left;line-height:25px;padding-left:15px;
              font-size:15px;"
              align="left"
            >
              全民纸巾大作战 <br />
              家具建材满999减300元 <br />
              黑科技冰箱，下单立减千元 <br />
            </p>
          </div>
        </div>
      </el-carousel>
    </div>
    <div class="section_second">
      <div class="section_second_header">
        <p class="section_second_header_img"></p>
        <div class="section_second_header_left">
          <p></p>
          <span class="">即将开拍</span>
          <span>期待已久的好货！</span>
          <span> </span>
        </div>
        <div class="section_second_header_right">
          <p>当前场次</p>
          <span class="section_second_header_right_hours">00</span>
          <span class="section_second_header_right_mao">：</span>
          <span class="section_second_header_right_minutes">00</span>
          <span class="section_second_header_right_mao">：</span>
          <span class="section_second_header_right_second">00</span>
          <p>后结束</p>
        </div>
      </div>
      <div class="section_second_list">
        <div class="swiper-container swiper_section_second_list_left">
          <div class="swiper-wrapper">
            <div class="swiper-slide">
              <ul>
                <li @click="toShopInfo()">
                  <img
                    src="~assets/images/section_second_list_img1.jpg"
                    alt=""
                  />
                  <p>
                    花王 (Merries) 妙而舒 纸尿裤 大号 L54片 尿不湿（9-14千克）
                    （日本官方直采） 花王 (Merries) 妙而舒 纸尿裤 大号 L54片
                    尿不湿（9-14千
                  </p>
                  <span>¥83.9</span><s>¥99.9</s>
                </li>
                <li>
                  <img
                    src="~assets/images/section_second_list_img2.jpg"
                    alt=""
                  />
                  <p>华为mate9 4GB+32GB版 月光银 移动联通电信4G手机 双卡</p>
                  <span>¥17.90</span><s>¥29.90</s>
                </li>
                <li>
                  <img
                    src="~assets/images/section_second_list_img3.jpg"
                    alt=""
                  />
                  <p>超能 植翠低泡洗衣液（鲜艳亮丽）2kg/袋装（新老包装随机</p>
                  <span>¥20.70</span><s>¥44.90</s>
                </li>
                <li>
                  <img
                    src="~assets/images/section_second_list_img4.jpg"
                    alt=""
                  />
                  <p>长城（GreatWall）红酒 特选5年橡木桶解百纳干红葡萄酒 整</p>
                  <span>¥399.00</span><s>¥599.00</s>
                </li>
                <li>
                  <img
                    src="~assets/images/section_second_list_img5.jpg"
                    alt=""
                  />
                  <p>惠普(HP)暗影精灵2代Pro 15.6英寸游戏笔记本电脑(i5-7300H</p>
                  <span>¥5999.00</span><s>¥6499.00</s>
                </li>
              </ul>
            </div>
            <div class="swiper-slide">
              <ul>
                <li>
                  <img
                    src="~assets/images/section_second_list_img6.jpg"
                    alt=""
                  />
                  <p>
                    Apple iMac 21.5英寸一体机（2017新款四核Core i5
                    处理器/8GB内存/1TB/RP555显卡/4K屏 MNDY2CH/A） Apple iMac
                    21.5英寸一体机（2017新款四核Core i5 处理
                  </p>
                  <span>¥9588.00</span><s>¥10288.00</s>
                </li>
                <li>
                  <img
                    src="~assets/images/section_second_list_img7.jpg"
                    alt=""
                  />
                  <p>中柏（Jumper）EZpad 4S Pro 10.6英寸二合一平板电脑（X5 z</p>
                  <span>¥848.00</span><s>¥899.00</s>
                </li>
                <li>
                  <img
                    src="~assets/images/section_second_list_img8.jpg"
                    alt=""
                  />
                  <p>
                    飞利浦（PHILIPS）电动牙刷HX6761/03亮白型成人充电式声波震动牙刷粉色
                    飞利浦（PHILIPS）电动牙刷HX6761/03亮白型成人充电式声波
                  </p>
                  <span>¥379.00</span><s>¥698.00</s>
                </li>
                <li>
                  <img
                    src="~assets/images/section_second_list_img9.jpg"
                    alt=""
                  />
                  <p>
                    美的(Midea) 258升 变频智能三门冰箱 一级能效 风冷无霜 中门
                  </p>
                  <span>¥3088.00</span><s>¥3299.00</s>
                </li>
                <li>
                  <img
                    src="~assets/images/section_second_list_img10.jpg"
                    alt=""
                  />
                  <p>【第二件减50元】蒙羊 内蒙古羔羊羊肋排 2.4斤</p>
                  <span>¥99.90</span><s>¥199.00</s>
                </li>
              </ul>
            </div>
          </div>
          <div class="swiper-button-prev second_list">
            <p></p>
          </div>
          <div class="swiper-button-next second_list">
            <p></p>
          </div>
        </div>
        <ul class="section_second_list_right">
          <li>
            <img
              src="~assets/images/section_second_list_right_img.jpg"
              alt=""
            />
          </li>
          <li>
            <img
              src="~assets/images/section_second_list_right_img.png"
              alt=""
            />
          </li>
          <div class="section_second_list_right_button">
            <p class="section_second_list_right_button_active"></p>
            <p></p>
          </div>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import "~/assets/css/GL.css";

export default {
  data() {
    return {
      imgUrls: [
        require("~/assets/images/notable1.jpg"),
        require("~/assets/images/notable2.jpg"),
        require("~/assets/images/notable3.jpg"),
        require("~/assets/images/notable4.jpg"),
        require("~/assets/images/notable5.jpg"),
      ],
    };
  },
  methods: {
    toShopInfo() {
      alert(1);
    },
  },
};
</script>

<style scope>
.main {
  text-align: center;
  border: 1px red solid;
}
.card {
  background: #ffffff;
  width: 270px;
  height: 300px;
  float: right;
  /* margin-right: 10px; */
  border: 1px blue solid;
}

.card span {
  margin: 20px;
  float: left;
  font-size: 14px;
  position: static;
  border-right: none;
  color: #e60012;
  font-size: 16px;
  font-weight: 600;
}
</style>
