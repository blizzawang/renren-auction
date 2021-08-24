<template>
  <div class="personal-main">
    <div class="personal-deposit">
      <h3><i>提现</i></h3>

      <div
        class="deposit-form"
        style="margin-top:0px;border-top:0px none;width:auto;height:240px;"
      >
        <el-steps :active="active" finish-status="success" style="margin: 0px">
          <el-step title="提交提现申请"></el-step>
          <el-step title="审核"></el-step>
          <el-step title="审核结果"></el-step>
        </el-steps>
        <div v-if="active === 1" style="margin-top:20px">
          <h6>填写提现金额</h6>
          <ul>
            <li>
              <span class="deposit-formleft">提现金额</span>
              <span class="deposit-formright">
                <input
                  v-model="fetchAmt"
                  type="text"
                  class="deposite-txt"
                  maxlength="10"
                />
                元
              </span>
            </li>
            <li>
              <span class="deposit-formleft">提现费用</span>
              <el-tooltip
                class="item"
                effect="dark"
                content="提现将收取0.1%的手续费"
                placement="top-start"
              >
                <em id="txfy" class="markicon fl"></em>
              </el-tooltip>
              <span class="deposit-formright deposit-formright1">
                <i> <label id="form:fee"> 0.00</label> </i>元
              </span>
            </li>
            <li>
              <input
                type="submit"
                value="马上提现"
                class="btn-depositok"
                @click="commitWithdraw()"
              />
            </li>
          </ul>
        </div>
        <div v-if="active === 2" class="deposit-form">
          <el-alert
            title="您的提现申请正在审批中......"
            type="warning"
            show-icon
            description="工作人员将在2小时内完成审核，审核时间为周一至周五8:00至20:00。"
            :closable="false"
          >
          </el-alert>
        </div>
      </div>
      <div class="deposit-tip" style="height: 110px;line-height: 24px;">
        <b>温馨提示：</b><br />
        1、为了您的资金安全，提现过程由工作人员审核后方可通过。<br />
        2、充值前请注意您的银行卡充值限额，以免造成不便。<br />
        3、为了您的资金安全，建议充值前进行实名认证。<br />
        4、如果充值遇到任何问题，请联系客服：4006-000-000。
      </div>
    </div>
  </div>
</template>
<script>
import cookie from "js-cookie";

export default {
  data() {
    return {
      fetchAmt: 0,
      active: 1,
      userId: 0,
    };
  },
  mounted() {
    this.getWithdrawStatus();
  },

  methods: {
    getWithdrawStatus() {
      this.getCookieUserId();
      this.$axios
        .$get("/login/withdraw/status/" + this.userId)
        .then((response) => {
          let status = response.data.status;
          if (status === 0) {
            // 未提交提现
            this.active = 1;
          } else if (status === 1) {
            // 提现审核中
            this.active = 2;
          } else if (status === 2) {
            // 提现审核通过
            this.active = 3;
          } else if (status === -1) {
            // 提现审核不通过
            this.active = 3;
          }
        });
    },
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
    commitWithdraw() {
      this.getCookieUserId();
      this.$message({
        message: "您的提现申请已提交，请耐心等待审核!",
        type: "success",
      });
      // 将步骤设置为第二步
      this.active = 1;
      this.$axios
        .$get(
          "/login/withdraw/auth/commitWithdraw/" +
            this.userId +
            "/" +
            this.fetchAmt
        )
        .then((response) => {
          // 刷新一下审核状态
          this.getWithdrawStatus();
        });
    },
  },
};
</script>
