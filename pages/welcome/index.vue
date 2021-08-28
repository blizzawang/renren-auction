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
          <span class="">正在拍卖</span>
          <span>期待已久的好货！</span>
          <span> </span>
        </div>
        <div class="section_second_header_right">
          <p>当前场次</p>
          <span class="section_second_header_right_hours">02</span>
          <span class="section_second_header_right_mao">：</span>
          <span class="section_second_header_right_minutes">00</span>
          <span class="section_second_header_right_mao">：</span>
          <span class="section_second_header_right_second">00</span>
          <p>后结束</p>
        </div>
      </div>
      <div class="section_second_list">
        <!-- 每5个商品一行 -->
        <div v-for="shopList in list" :key="shopList.index">
          <div class="swiper-container swiper_section_second_list_left">
            <div class="swiper-wrapper">
              <div class="swiper-slide">
                <ul>
                  <li
                    v-for="shop in shopList"
                    :key="shop.shopId"
                    @click="toShopInfo(shop.shopId)"
                  >
                    <img :src="shop.shopIndexImage" alt="" />
                    <p>
                      {{ shop.shopName }}
                    </p>
                    <span>¥{{ shop.shopStartPrice }}</span
                    ><s>¥{{ shop.shopStartPrice + 20 }}</s>
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
        </div>
        <!-- <ul class="section_second_list_right">
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
        </ul> -->
      </div>
    </div>
  </div>
</template>

<script>
import "~/assets/css/GL.css";

export default {
  data() {
    return {
      count: 0,
      // shopList: [],
      list: [],
      imgUrls: [
        require("~/assets/images/notable1.jpg"),
        require("~/assets/images/notable2.jpg"),
        require("~/assets/images/notable3.jpg"),
        require("~/assets/images/notable4.jpg"),
        require("~/assets/images/notable5.jpg"),
      ],
    };
  },
  created() {
    this.getShopInfo();
  },
  methods: {
    // 获取即将开拍的拍品信息
    getShopInfo() {
      this.$axios.$get("/login/shop/list").then((response) => {
        let list = response.data.list;
        // 将拍品集合以每6个为一组进行分组
        var tempList = [];
        var newList = [];
        var count = 0;
        let flag = list.length % 6;
        for (var i = 0; i < list.length; ++i) {
          tempList.push(list[i]);
          count++; // 计数器加1
          if (count === 6) {
            // 6个一组
            // 将tempList放入newList
            newList.push(tempList);
            count = 0; // 重置计数器
            tempList = []; // 清空tempList
          }
        }
        tempList = [];
        if (count !== 0) {
          // 若count不等于0，则说明等分不均匀，还有余下count个数的拍品
          for (var i = 0; i < count; ++i) {
            tempList.push(list[list.length - 1 - i]);
          }
        }
        newList.push(tempList.reverse());
        this.list = newList;
        console.log(newList);
      });
    },
    // 点击某个拍品跳转至对应的详细信息页面
    toShopInfo(shopId) {
      this.$router.push({
        path: "/shop/detail",
        query: {
          id: shopId,
        },
      });
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
