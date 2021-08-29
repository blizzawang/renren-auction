<template>
  <div style="float:left;margin-top:10px">
    <el-table :data="list" style="width: 940px">
      <el-table-column
        prop="shopName"
        label="拍品名称"
        width="350"
        align="center"
      >
      </el-table-column>
      <el-table-column prop="offerCount" label="出价次数" align="center">
      </el-table-column>
      <el-table-column prop="shopStatus" label="拍品状态" align="center">
        <template slot-scope="scope">
          <el-tag v-if="scope.row.shopStatus === 1" type="warning" size="mini">
            拍卖中
          </el-tag>
          <el-tag v-if="scope.row.shopStatus === 2" type="success" size="mini">
            已成交
          </el-tag>
          <el-tag v-if="scope.row.shopStatus === -1" type="danger" size="mini">
            撤拍
          </el-tag>
        </template>
      </el-table-column>
      <el-table-column prop="commitStatus" label="成交状态" align="center">
        <template slot-scope="scope">
          <el-tag
            v-if="scope.row.commitStatus === 1"
            type="success"
            size="mini"
          >
            成功
          </el-tag>
          <el-tag v-if="scope.row.commitStatus === 2" type="danger" size="mini">
            失败
          </el-tag>
        </template>
      </el-table-column>
      <el-table-column label="操作" width="200" align="center">
        <template slot-scope="scope">
          <el-button
            type="primary"
            size="mini"
            @click="toOfferDetailRecord(scope.row.id)"
            >查看详情</el-button
          >
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
      list: [],
    };
  },
  mounted() {
    // 获取出价记录数据
    this.fetchData();
  },
  methods: {
    // 点击查看详情按钮，跳转至指定商品的出价记录表
    toOfferDetailRecord(shopId) {
      this.$router.push({
        path: "/user/offerDetail",
        query: {
          id: shopId,
        },
      });
    },
    // 获取出价记录数据
    fetchData() {
      this.getCookieUserId();
      this.$axios
        .get(
          `/login/offerRecord/list/${this.userId}/${this.page}/${this.limit}`
        )
        .then((response) => {
          this.list = response.data.data.pageModel.list;
          this.total = response.data.data.pageModel.total;
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
  },
};
</script>

<style></style>
