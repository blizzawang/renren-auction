<template>
  <div class="personal-main">
    <div class="pmain-profile">
      <div class="personal-money">
        <h3><i>发布拍卖</i></h3>
        <el-form ref="form" label-width="90px" style="padding-top:20px;">
          <el-form-item label="拍卖模式">
            <el-radio-group v-model="shopDetail.shopModel">
              <el-radio label="竞拍" value="竞拍"></el-radio>
              <el-radio label="竞标" value="竞标"></el-radio>
            </el-radio-group>
            <br />
            <label style="color:#AAAAAA"
              >竞拍：规定时间，多次出价，价高者得。竞标：规定时间，一次出价，价高者得。</label
            >
          </el-form-item>
          <el-form-item label="成交模式" v-if="shopDetail.shopModel === '竞拍'">
            <el-radio-group v-model="shopDetail.commitModel">
              <el-radio label="普通模式" value="普通模式"></el-radio>
              <el-radio label="即时成交" value="即时成交"></el-radio>
            </el-radio-group>
          </el-form-item>
          <el-form-item
            label="即时成交"
            v-if="shopDetail.commitModel === '即时成交'"
          >
            <el-input placeholder="请输入成交价"></el-input>
            <label style="color:#AAAAAA"
              >当用户出价等于或大于成交价时，拍卖结束，用户以成交价拍得拍品。</label
            >
          </el-form-item>
          <el-form-item label="开始时间">
            <el-date-picker
              v-model="shopDetail.startTime"
              type="datetime"
              placeholder="选择日期时间"
              align="right"
            >
            </el-date-picker>
          </el-form-item>
          <el-form-item label="结束时间">
            <el-date-picker
              v-model="shopDetail.endTime"
              type="datetime"
              placeholder="选择日期时间"
              align="right"
            >
            </el-date-picker>
          </el-form-item>
          <el-form-item label="起拍价">
            <el-input v-model="shopDetail.shopStartPrice"></el-input>
          </el-form-item>
          <el-form-item label="保留价">
            <el-input v-model="shopDetail.shopRetainPrice"></el-input>
          </el-form-item>
          <el-form-item label="运费">
            <el-input v-model="shopDetail.shopFreight"></el-input>
          </el-form-item>
          <el-form-item label="价格浮动" v-if="shopDetail.shopModel === '竞拍'">
            <el-radio-group v-model="price">
              <el-radio label="定额浮动" value="定额浮动"></el-radio>
              <el-radio label="阶梯浮动" value="阶梯浮动"></el-radio>
            </el-radio-group>
            <br />
            <label style="color:#AAAAAA">浮动为用户每次最低加价</label>
          </el-form-item>
          <el-form-item label="定额浮动" v-if="price === '定额浮动'">
            <el-input
              placeholder="每次买家出价固定加价多少"
              v-model="shopDetail.shopAddPrice"
            ></el-input>
          </el-form-item>
          <el-form-item label="阶梯浮动" v-if="price === '阶梯浮动'">
            <el-input placeholder="每次买家出价固定加价多少"></el-input>
          </el-form-item>
          <el-form-item label="浮动阈值" v-if="price === '阶梯浮动'">
            <el-input
              placeholder="当前价格大于等于多少金额时，价格进行浮动"
            ></el-input>
          </el-form-item>
          <el-form-item label="浮动比例" v-if="price === '阶梯浮动'">
            <el-input placeholder="浮动金额占当前拍品出价的百分比"></el-input>
          </el-form-item>
          <el-form-item label="浮动上限" v-if="price === '阶梯浮动'">
            <el-input placeholder="浮动金额最高不能超过多少金额"></el-input>
          </el-form-item>
          <el-form-item label="保证金">
            <el-radio-group v-model="money">
              <el-radio label="定额收取" value="定额收取"></el-radio>
              <el-radio
                label="起拍价比例收取"
                value="起拍价比例收取"
              ></el-radio>
            </el-radio-group>
          </el-form-item>
          <el-form-item label="定额收取" v-if="money === '定额收取'">
            <el-input
              placeholder="固定收取多少的保证金"
              v-model="shopDetail.earnestMoney"
            ></el-input>
          </el-form-item>
          <el-form-item label="比例收取" v-if="money === '起拍价比例收取'">
            <el-input placeholder="按照起拍价的百分比收取保证金"></el-input>
          </el-form-item>
          <el-form-item label="商品小图"
            ><el-upload
              class="upload-demo"
              :action="uploadUrl"
              :on-remove="onUploadRemove"
              :on-success="onUploadSuccessShopSmallImage"
              :data="{ module: 'small' }"
              multiple
              :limit="1"
            >
              <el-button size="small" type="primary">点击上传</el-button>
              <div slot="tip" class="el-upload__tip">
                只能上传jpg/png文件，且不超过500kb
              </div>
            </el-upload>
          </el-form-item>
          <el-form-item label="商品大图"
            ><el-upload
              class="upload-demo"
              :action="uploadUrl"
              :on-remove="onUploadRemove"
              :on-success="onUploadSuccessShopBigImage"
              :data="{ module: 'big' }"
              multiple
              :limit="1"
            >
              <el-button size="small" type="primary">点击上传</el-button>
              <div slot="tip" class="el-upload__tip">
                只能上传jpg/png文件，且不超过500kb
              </div>
            </el-upload>
          </el-form-item>
          <el-form-item style="float:right">
            <el-button type="primary" @click="next">
              发布拍卖
            </el-button>
          </el-form-item>
        </el-form>
      </div>
    </div>
  </div>
</template>

<script>
import cookie from "js-cookie";

export default {
  data() {
    let BASE_API = process.env.BASE_API;

    return {
      userId: 0, // 当前用户id
      uploadUrl: BASE_API + "/oss/file/upload", // 文件上传地址
      shop: {},
      shopDetail: {
        shopModel: "竞拍",
        commitModel: "普通模式",
      },
      value2: "",
      money: "定额收取",
      price: "定额浮动",
    };
  },
  mounted() {
    // 接收上一个页面传递过来的参数
    this.getAddPageParam();
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
    // 接收上一个页面传递过来的参数
    getAddPageParam() {
      this.shopDetail.shopName = this.$route.query.shopName;
      this.shopDetail.shopPrice = this.$route.query.shopPrice;
      this.shopDetail.shopDescription = this.$route.query.shopDescription;
      this.shopDetail.shopIndexImage = this.$route.query.indexImageUrl;
    },
    // 点击发布拍卖，若发布成功则跳转至拍卖列表页
    next() {
      this.$message({
        message: "发布成功",
        type: "success",
      });
      this.getCookieUserId();
      // 设置卖家id
      this.shopDetail.sellerId = this.userId;
      // 发送请求发布商品
      this.$axios
        .$post("/login/shop/put/shop", this.shopDetail)
        .then((response) => {
          // 发布完成后跳转至商品列表页
          this.$router.push("/user/auctionList");
        });
    },
    // 文件上传
    onUploadSuccessShopSmallImage(response, file) {
      this.shopDetail.shopSmallImage = response.data.url;
    },
    onUploadSuccessShopBigImage(response, file) {
      this.shopDetail.shopBigImage = response.data.url;
    },
    // 调用后台接口删除指定的文件
    onUploadRemove(file) {
      this.$axios
        .$delete("/oss/file/remove?url=" + file.response.data.url)
        .then((response) => {
          console.log("远程删除文件成功");
        });
    },
  },
};
</script>

<style></style>
