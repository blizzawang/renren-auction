<template>
  <div class="personal-main">
    <div class="personal-money">
      <h3><i>资金记录</i></h3>

      <div class="personal-moneylist" style="margin-top: 40px;">
        <div class="pmain-contitle">
          <span class="pmain-title1 fb" style="width: 330px;">
            出价时间
          </span>
          <span class="pmain-title2 fb" style="width: 220px;">
            拍品名称
          </span>
          <span class="pmain-title3 fb" style="width: 200px;">
            出价金额
          </span>
          <span style="width:150px"></span>
        </div>
        <ul>
          <li v-for="record in list" :key="record.id" style="width:100%">
            <span class="pmain-title1 pmain-height" style="width: 320px;">
              {{ record.createTime }}
            </span>
            <span
              class="pmain-title2 pmain-height"
              style="width: 245px;white-space:nowrap;text-overflow:ellipsis;overflow:hidden;"
            >
              {{ record.shopName }}
            </span>
            <span class="pmain-title3 pmain-height" style="width: 180px;">
              {{ record.price }}
            </span>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import cookie from "js-cookie";

export default {
  data() {
    return {
      userId: 0,
      list: [],
    };
  },
  mounted() {
    this.fetchData();
  },
  methods: {
    // 获取数据
    fetchData() {
      this.getCookieUserId();
      let shopId = this.$route.query.id;
      this.$axios
        .get(`/login/offerRecord/list/by/${this.userId}/${shopId}`)
        .then((response) => {
          this.list = response.data.data.list;
        });
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
  },
};
</script>
