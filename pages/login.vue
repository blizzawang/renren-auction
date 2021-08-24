<template>
  <!--登录-->
  <div class="wrap">
    <div class="tdbModule loginPage">
      <div class="registerTitle">用户登录</div>
      <div class="registerCont">
        <ul>
          <li>
            <span class="dis"></span>
            <input v-model="userInfo.userType" type="radio" value="1" />
            买家
            <input v-model="userInfo.userType" type="radio" value="2" />
            卖家
          </li>
          <li>
            <span class="dis">手机号：</span>
            <input class="input" v-model="userInfo.phone" />
          </li>

          <li>
            <span class="dis">密码：</span>
            <input class="input" v-model="userInfo.password" type="password" />
          </li>
          <li class="btn">
            <button @click="login()" :class="{ disabled: !isValid }">
              立即登录
            </button>
          </li>
          <li style="margin-left: 160px">
            <a id="weiLogin" href="#"
              ><img src="~/assets/images/wechat.png" style="float:left" /><span
                style="float:left;font-size:15px;margin-left:5px;margin-top:5px;color:#1296db"
                >微信登录</span
              >
            </a>
          </li>
          <li>
            <a
              id="weiLogin"
              href="https://gitee.com/oauth/authorize?client_id=69413b43802b19c4bc5072dbd67de09acc4e0fe50a7bde04f4b38f8eb7b7fafe&redirect_uri=http%3A%2F%2Flocalhost%3A8080%2Fsuccess&response_type=code"
              ><img src="~/assets/images/gitee2.png" style="float:left" /><span
                style="float:left;font-size:15px;margin-left:5px;margin-top:5px;color:#1296db"
                >Gitee登录</span
              >
            </a>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import "~/assets/css/register.css";
import cookie from "js-cookie";

export default {
  data() {
    return {
      userInfo: {
        userType: 1,
      },
      isValid: true, //表单校验是否成功
    };
  },

  methods: {
    //登录
    login() {
      this.$axios
        .$post("/login/userInfo/login", this.userInfo)
        .then((response) => {
          if (response.message === "登录成功") {
            this.$message.success(response.message);
            cookie.set("userInfo", response.data.userInfo);
            window.location.href = "/user";
          } else {
            this.$message.error(response.message);
          }
        });
    },
  },
};
</script>
<style scoped></style>
