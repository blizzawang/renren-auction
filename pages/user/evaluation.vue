<template>
  <div>
    <el-input
      type="textarea"
      :rows="2"
      placeholder="请写下对宝贝的感受吧，您的意见对他人帮助很大哦！"
      v-model="textarea"
      style="float:left;width:920px;margin-top:10px"
    >
    </el-input>
    <el-rate v-model="value" show-text :texts="texts" st> </el-rate>
    <el-button
      type="success"
      style="float:right;margin-top:30px;margin-right:20px"
      >发表</el-button
    >
  </div>
</template>

<script>
export default {
  data() {
    return {
      userId: 0,
      shopId: 0,
      textarea: "",
      texts: ["2.0", "4.0", "6.0", "8.0", "10.0"], // 规定6.0以下的评分为差评
      value: null,
    };
  },
  mounted() {
    this.getIds();
  },
  methods: {
    // 获取上一页传递过来的用户和商品id
    getIds() {
      this.userId = this.$route.query.userId;
      this.shopId = this.$route.query.shopId;
      console.log(this.userId);
      console.log(this.shopId);
    },
  },
};
</script>

<style></style>
