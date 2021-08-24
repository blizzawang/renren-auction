<template>
  <div>
    <el-menu
      style="width:800px;margin:0 auto"
      :default-active="activeIndex"
      class="el-menu-demo"
      mode="horizontal"
      text-color="#000000"
      @select="handleSelect"
      router
    >
      <el-menu-item index="/welcome" style="margin-left:60px">
        首页
      </el-menu-item>
      <el-menu-item index="/welcome/NewProduct">最新拍品</el-menu-item>
      <el-menu-item index="/welcome/JianLou">0元捡漏</el-menu-item>
      <el-menu-item index="/welcome/YiKouJia">限时一口价</el-menu-item>
      <el-menu-item index="/welcome/ZhuanChang">专场拍卖</el-menu-item>
      <el-menu-item index="/welcome/PaiMaiHui">拍卖会</el-menu-item>
      <el-menu-item index="/welcome/HaoDian">优质好店</el-menu-item>
    </el-menu>
    <nuxt-child keep-alive />
  </div>
</template>

<script>
import "~/assets/css/about.css";

export default {
  data() {
    return {
      activeIndex: this.$route.path,
      selfstyle: {},
    };
  },
  methods: {
    handleSelect(key, keyPath) {
      // console.log(key, keyPath);
    },
  },
};
</script>
<style scoped></style>
