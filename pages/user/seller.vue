<template>
  <div class="personal-main">
    <div class="personal-pay">
      <h3><i>申请成为卖家</i></h3>

      <el-steps :active="active" style="margin: 40px">
        <el-step title="填写申请资料"></el-step>
        <el-step title="提交平台审核"></el-step>
        <el-step title="等待认证结果"></el-step>
      </el-steps>

      <div v-if="active === 1" class="user-borrower">
        <h6>个人基本信息</h6>
        <el-form label-width="120px">
          <el-form-item label="店铺名称">
            <el-col :span="5">
              <el-input v-model="seller.name" />
            </el-col>
          </el-form-item>
          <el-form-item label="手机号码">
            <el-col :span="5">
              <el-input v-model="seller.phone" />
            </el-col>
          </el-form-item>
          <el-form-item label="身份证号">
            <el-col :span="5">
              <el-input v-model="seller.idCard" />
            </el-col>
          </el-form-item>
        </el-form>

        <h6>联系人信息</h6>
        <el-form label-width="120px">
          <el-form-item label="联系人姓名">
            <el-col :span="5">
              <el-input v-model="seller.contactsName" />
            </el-col>
          </el-form-item>
          <el-form-item label="联系人手机">
            <el-col :span="5">
              <el-input v-model="seller.contactsMobile" />
            </el-col>
          </el-form-item>
          <el-form-item label="联系人关系">
            <el-select
              v-model="seller.contactsRelation"
              placeholder="请选择"
              clearable
            >
              <el-option label="父母" value="0" />
              <el-option label="配偶" value="1" />
              <el-option label="其它关系" value="2" />
            </el-select>
            <!-- <el-select v-model="seller.contactsRelation">
              <el-option
                v-for="item in contactsRelationList"
                :key="item.value"
                :label="item.name"
                :value="item.value"
              />
            </el-select> -->
          </el-form-item>
        </el-form>

        <h6>身份认证信息</h6>
        <el-form label-width="120px">
          <el-form-item label="身份证人像面">
            <el-upload
              :on-success="onUploadSuccessIdCard1"
              :on-remove="onUploadRemove"
              :multiple="false"
              :action="uploadUrl"
              :data="{ module: 'idCard1' }"
              :limit="1"
              list-type="picture-card"
            >
              <i class="el-icon-plus"></i>
            </el-upload>
          </el-form-item>
          <el-form-item label="身份证国徽面">
            <el-upload
              :on-success="onUploadSuccessIdCard2"
              :on-remove="onUploadRemove"
              :multiple="false"
              :action="uploadUrl"
              :data="{ module: 'idCard2' }"
              :limit="1"
              list-type="picture-card"
            >
              <i class="el-icon-plus"></i>
            </el-upload>
          </el-form-item>
          <el-form-item label="手持身份证像">
            <el-upload
              :on-success="onUploadSuccessIdCard3"
              :on-remove="onUploadRemove"
              :multiple="false"
              :action="uploadUrl"
              :data="{ module: 'idCard3' }"
              :limit="1"
              list-type="picture-card"
            >
              <i class="el-icon-plus"></i>
            </el-upload>
          </el-form-item>
        </el-form>

        <h6>其它信息</h6>
        <el-form label-width="120px">
          <el-form-item label="营业执照">
            <el-tooltip
              class="item"
              effect="dark"
              content="只有企业店铺才需要上传营业执照"
              placement="top-start"
            >
              <em id="txfy" class="markicon fl"></em>
            </el-tooltip>
            <el-upload
              :on-success="onUploadSuccessLicense"
              :on-remove="onUploadRemove"
              :multiple="false"
              :action="uploadUrl"
              :data="{ module: 'house' }"
              list-type="picture-card"
            >
              <i class="el-icon-plus"></i>
            </el-upload>
          </el-form-item>

          <el-form-item label="社会信用代码">
            <el-tooltip
              class="item"
              effect="dark"
              content="只有企业店铺才需要填写社会信用代码"
              placement="top-start"
            >
              <em id="txfy" class="markicon fl"></em>
            </el-tooltip>
            <el-col :span="5">
              <el-input v-model="seller.contactsName" />
            </el-col>
          </el-form-item>
        </el-form>

        <el-form label-width="120px">
          <el-form-item>
            <el-button
              type="primary"
              :disabled="submitBtnDisabled"
              @click="save"
            >
              提交
            </el-button>
          </el-form-item>
        </el-form>
      </div>

      <div v-if="active === 2">
        <div style="margin-top:40px;">
          <el-alert
            title="您的成为卖家申请已成功提交，请耐心等待"
            type="warning"
            show-icon
            :closable="false"
          >
            我们将在24小时内对您所提交的材料进行核查，请保证材料的真实性，审核时间为周一至周五8:00至20:00。
          </el-alert>
        </div>
      </div>

      <div v-if="active === 3">
        <div style="margin-top:40px;">
          <el-alert
            v-if="status === 2"
            title="您的认证审批已通过"
            type="success"
            show-icon
            :closable="false"
          >
          </el-alert>

          <NuxtLink to="/user/add" v-if="status === 2">
            <el-button style="margin-top:20px;" type="success">
              上架拍品
            </el-button>
          </NuxtLink>

          <el-alert
            v-if="status === -1"
            title="您的认证审批未通过"
            type="error"
            show-icon
            :closable="false"
          >
          </el-alert>
          <el-button
            style="margin-top:20px;"
            type="success"
            v-if="status === -1"
            @click="changeActive"
          >
            重新申请
          </el-button>
        </div>
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
      submitBtnDisabled: false, // 防止表单重复提交
      userId: 0,
      status: 0, //审核状态
      // 卖家申请信息
      seller: {
        sellerAttachList: [], // 上传的图片内容
      },
      active: 0,
      uploadUrl: BASE_API + "/oss/file/upload", // 文件上传地址
    };
  },
  // 页面创建时调用接口获取卖家申请审核状态
  created() {
    this.getCookieUserId();
  },
  mounted() {
    // 获取审核状态
    this.getSellerAuditStatus();
  },
  methods: {
    // 改变当前步骤为第一步，重新申请
    changeActive() {
      this.active = 1;
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
    // 获取卖家申请审核状态
    getSellerAuditStatus() {
      this.getCookieUserId();
      this.$axios
        .$get("/login/audit/seller/get/status/" + this.userId)
        .then((response) => {
          this.status = response.data.status;
          if (this.status === 0) {
            // 未提交
            this.active = 1;
          } else if (this.status === 1) {
            // 审核中
            this.active = 2;
          } else if (this.status === 2) {
            // 审核通过
            this.active = 3;
          } else if (this.status === -1) {
            // 审核不通过
            this.active = 3;
          }
        });
    },
    // 提交审核资料
    save() {
      this.getCookieUserId();
      // this.submitBtnDisabled = true;
      this.$axios
        .$post("/login/audit/seller/auth/save/" + this.userId, this.seller)
        .then((response) => {
          // 刷新一下审核状态
          this.getSellerAuditStatus();
          this.active = 1; // 进入流程的下一步
        });
    },
    // 上传身份证人像面
    onUploadSuccessIdCard1(response, file) {
      this.onUploadSuccess(response, file, "idCard1");
    },
    // 上传身份证国徽面
    onUploadSuccessIdCard2(response, file) {
      this.onUploadSuccess(response, file, "idCard2");
    },
    // 上传手持身份证像
    onUploadSuccessIdCard3(response, file) {
      this.onUploadSuccess(response, file, "idCard3");
    },
    onUploadSuccessLicense(response, file) {
      this.onUploadSuccess(response, file, "license");
    },
    onUploadSuccess(response, file, type) {
      if (response.code === 200) {
        // 业务成功，填充seller.sellerAttachList列表数据
        this.seller.sellerAttachList.push({
          imageName: file.name,
          imageUrl: response.data.url,
          imageType: type,
        });
      } else {
        // 业务失败
        this.$message.error(response.message);
      }
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
