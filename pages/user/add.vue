<template>
  <div class="personal-main">
    <div class="pmain-profile">
      <div class="personal-money">
        <h3><i>添加拍品</i></h3>
        <el-form ref="form" label-width="90px" style="padding-top:20px;">
          <el-form-item label="拍品名称">
            <el-input v-model="shop.shopName"></el-input>
          </el-form-item>
          <el-form-item label="拍品价格">
            <el-input v-model="shop.shopPrice"></el-input>
          </el-form-item>
          <el-form-item label="拍品描述">
            <el-input v-model="shop.shopDescription"></el-input>
          </el-form-item>
          <el-form-item label="首页展示图">
            <el-upload
              class="upload-demo"
              :action="uploadUrl"
              :on-remove="onUploadRemove"
              :on-success="onUploadSuccessIndexImg"
              :data="{ module: 'index' }"
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
              下一步
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
      uploadUrl: BASE_API + "/oss/file/upload", // 文件上传地址
      shop: {},
      address: [],
      options: null,
    };
  },
  methods: {
    // 点击下一步，设置拍卖的详细信息
    next() {
      // console.log(this.shop);
      this.$router.push({
        path: "/user/auctionDetail",
        query: this.shop,
      });
    },
    // 文件上传
    onUploadSuccessIndexImg(response, file) {
      this.shop.indexImageUrl = response.data.url;
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
