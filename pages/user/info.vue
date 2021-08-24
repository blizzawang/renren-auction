<template>
  <div class="personal-main">
    <div class="pmain-profile">
      <div class="personal-money">
        <h3><i>个人信息</i></h3>
        <el-form ref="form" label-width="80px" style="padding-top:20px;">
          <el-form-item label="昵称">
            <el-input v-model="userIndexVO.nickName"></el-input>
          </el-form-item>
          <el-form-item label="性别">
            <el-radio-group v-model="userIndexVO.sex">
              <el-radio label="男" value="男"></el-radio>
              <el-radio label="女" value="女"></el-radio>
            </el-radio-group>
          </el-form-item>
          <el-form-item label="生日">
            <el-col :span="11">
              <el-date-picker
                type="date"
                placeholder="选择日期"
                style="width: 100%;"
                v-model="userIndexVO.birthday"
              ></el-date-picker>
            </el-col>
          </el-form-item>
          <el-form-item label="所在地">
            <el-cascader
              placeholder="试试搜索：北京"
              v-model="userIndexVO.address"
              :options="options"
              filterable
            ></el-cascader>
          </el-form-item>
          <el-form-item label="详细地址">
            <el-input v-model="userIndexVO.detailAddress"></el-input>
          </el-form-item>
          <el-form-item label="个性签名">
            <el-input v-model="userIndexVO.signature"></el-input>
          </el-form-item>
          <el-form-item>
            <el-button type="primary" @click="onSubmit">保存修改</el-button>
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
    return {
      userIndexVO: {},
      address: [],
      options: null,
    };
  },
  mounted() {
    this.fetchUserData();
    this.getRegionData();
  },
  methods: {
    getRegionData() {
      this.$axios.$get("/login/userAddress/address").then((response) => {
        let json = response.data.data;
        let obj = JSON.parse(json);
        this.options = obj;
      });
    },
    fetchUserData() {
      // 判断当前Cookie中是否包含用户信息
      let userInfo = cookie.get("userInfo");
      if (!userInfo) {
        this.userInfo = null;
        return;
      }
      userInfo = JSON.parse(userInfo);
      this.userIndexVO = userInfo;
    },
    onSubmit() {
      // 提交用户修改的数据，对用户信息进行更新
      let userInfo = JSON.parse(cookie.get("userInfo"));
      // 从cookie中取出当前用户的手机号作为标识
      this.userIndexVO.phone = userInfo.phone;
      this.$axios
        .$post("/login/userInfo/updateUserInfo", this.userIndexVO)
        .then((response) => {
          // 更新一下cookie中的用户信息
          cookie.set("userInfo", this.userIndexVO);
        });
    },
  },
};
</script>

<style></style>
