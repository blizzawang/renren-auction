<template>
  <div style="float:left;margin-top:10px">
    <el-table :data="shopList" style="width: 940px">
      <el-table-column label="拍品" width="300" align="center">
        <template slot-scope="scope">
          <div>
            <img style="float:left" :src="scope.row.shopIndexImage" />
            <p style="float:left;margin-top:20px;margin-left:10px">
              <p
                style="overflow: hidden;white-space: nowrap;text-overflow: ellipsis;"
              >
                {{ scope.row.shopName }}
              </p>
              <br />
              <br />
              <br />
              保证金：¥{{ scope.row.earnestMoney }}
            </p>
          </div>
        </template>
      </el-table-column>
      <el-table-column
        prop="shopCurrentPrice"
        label="当前价"
        width="120"
        align="center"
      >
      </el-table-column>
      <el-table-column prop="status" label="状态" width="100" align="center">
        <template slot-scope="scope">
          <el-tag v-if="scope.row.status === '1'" type="warning" size="mini">
            拍卖中
          </el-tag>
          <el-tag v-if="scope.row.status === '2'" type="success" size="mini">
            已成交
          </el-tag>
          <el-tag v-if="scope.row.status === '-1'" type="danger" size="mini">
            撤拍
          </el-tag>
        </template>
      </el-table-column>
      <el-table-column
        prop="startTime"
        label="开拍时间"
        width="100"
        align="center"
      >
      </el-table-column>
      <el-table-column
        prop="endTime"
        label="结束时间"
        width="100"
        align="center"
      >
      </el-table-column>
      <el-table-column label="操作" width="200" align="center">
        <template slot-scope="scope">
          <el-button
            v-if="scope.row.status === '1'"
            type="primary"
            size="mini"
            @click="chepai(scope.row.id)"
            >撤拍</el-button
          >
          <el-button v-if="scope.row.status === '-1'" type="danger" size="mini"
            >再次拍卖</el-button
          >
          <el-button v-if="scope.row.status === '2'" type="warning" size="mini"
            >查看订单</el-button
          >
          <el-button type="info" size="mini">详情</el-button>
        </template>
      </el-table-column>
    </el-table>

    <!-- 分页组件 -->
    <el-pagination
      :current-page="page"
      :total="total"
      :page-size="limit"
      :page-sizes="[2, 5, 10]"
      style="padding: 30px 0; "
      layout="total, sizes, prev, pager, next, jumper"
      @size-change="changePageSize"
      @current-change="changeCurrentPage"
    />
  </div>
</template>

<script>
import cookie from "js-cookie";

export default {
  data() {
    return {
      userId: 0, //用户id
      total: 0, // 数据库中的总记录数
      page: 1, // 默认页码
      limit: 2, // 每页记录数
      shopList: [],
    };
  },
  created() {
    // 获取商品列表
    this.fetchData();
  },
  methods: {
    // 点击撤拍按钮，将正在拍卖的商品撤下来，此时状态改为撤拍(-1)
    chepai(shopId) {
      this.$confirm(
        "您真的打算将该商品撤拍吗？撤拍将立即结束拍卖，并解冻您和买家的保证金。",
        "提示",
        {
          confirmButtonText: "确定",
          cancelButtonText: "取消",
          type: "warning",
        }
      )
        .then(() => {
          this.$message({
            type: "success",
            message: "撤拍成功",
          });
          // 修改商品状态为撤拍
          this.$axios.get(`login/shop/status/-1/${shopId}`).then((response) => {
            // 修改成功后刷新数据
            this.fetchData();
          });
        })
        .catch(() => {
          this.$message({
            type: "info",
            message: "已取消",
          });
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
    fetchData() {
      this.getCookieUserId();
      this.$axios
        .get(`/login/shop/list/${this.userId}/${this.page}/${this.limit}`)
        .then((response) => {
          // this.shopList = response.data.pageModel.list;
          this.total = response.data.data.pageModel.total;
          this.shopList = response.data.data.pageModel.list;
          console.log(this.total)
        });
    },

    // 每页记录数改变，size：回调参数，表示当前选中的“每页条数”
    changePageSize(size) {
      this.limit = size;
      this.fetchData();
    },

    // 改变页码，page：回调参数，表示当前选中的“页码”
    changeCurrentPage(page) {
      this.page = page;
      this.fetchData();
    },

    // 重置表单
    resetData() {
      this.keyword = "";
      this.fetchData();
    },
  },
};
</script>
