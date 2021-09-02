<template>
  <div style="float:left;margin-top:10px">
    <el-table :data="list" style="width: 940px">
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
              {{ scope.row.storeName }}
            </p>
          </div>
        </template>
      </el-table-column>
      <el-table-column
        prop="buyerName"
        label="收货人"
        width="120"
        align="center"
      >
      </el-table-column>
       <el-table-column
        prop="payPrice"
        label="金额"
        width="120"
        align="center"
      >
      </el-table-column>
      <el-table-column prop="shopStatus" label="物流状态" width="100" align="center">
        <template slot-scope="scope">
          <el-tag v-if="scope.row.shopStatus === 0" type="warning" size="mini">
            未发货
          </el-tag>
          <el-tag v-if="scope.row.shopStatus === 1" type="warning" size="mini">
            运送中
          </el-tag>
          <el-tag v-if="scope.row.shopStatus === 2" type="success" size="mini">
            已收货
          </el-tag>
          <el-tag v-if="scope.row.shopStatus === -1" type="danger" size="mini">
            退货中
          </el-tag>
          <br/>
            <img src="~@/assets/images/logistics.png" style="float:left;margin-top:5px;margin-left:5px"/>
            <el-tooltip placement="left" effect="light">
            <div slot="content">
                <strong>普通快递 运单号:390085324974</strong>
                <br/><br/><br/>
                [北京市] 在北京昌平区南口公司进行签收扫描
                <br/>
                快件已被拍照(您 的快件已签收,感谢您使用韵达快递)签收
                <br/><br/><br/><br/>
                [北京市] 在北京昌平区南口公司进行签收扫描
                <br/>
                快件已被拍照(您 的快件已签收,感谢您使用韵达快递)签收
                <br/><br/><br/><br/>
                [北京昌平区南口公司] 在北京昌平区南口公司进行派件扫描
                <br/><br/><br/><br/>
                [北京市] 在北京昌平区南口公司进行派件扫描
                <br/><br/><br/><br/>
                派送业务员:业务员;联系电话:18xxxxxxxx
            </div>
                <span style="font-family: '微软雅黑';float:left;margin-top:10px;margin-left:5px;">跟踪</span>
            </el-tooltip>
        </template>
      </el-table-column>
      <el-table-column label="操作" width="200" align="center">
        <template slot-scope="scope">
          <el-button
            v-if="scope.row.shopStatus === 1"
            type="primary"
            size="mini"
            @click="confirm(scope.row.id)"
            >确认收货</el-button
          >
          <el-button
            v-if="scope.row.shopStatus === 2"
            type="warning"
            size="mini"
            @click="pingjia(scope.row.shopId)"
            >去评价</el-button
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
            list:[],
            userId:0,
            total: 0, // 数据库中的总记录数
            page: 1, // 默认页码
            limit: 2, // 每页记录数
        }
    },
    mounted() {
        this.getBuyRecordData();
    },
    methods: {
        // 点击评价按钮，跳转至去评价页面
        pingjia(shopId){
            this.$router.push({
                path:"/user/evaluation",
                query:{
                    userId:this.userId,
                    shopId:shopId
                }
            })
        },
        // 点击确认收货按钮，将商品状态修改为已收货
        confirm(id){
            this.$axios.get("/login/buyRecord/status/" + id).then(response=>{
                // 刷新数据
                this.getBuyRecordData();
            })
        },
        // 获取买家的购买记录数据
        getBuyRecordData(){
            this.getCookieUserId();
            this.$axios.get(`/login/buyRecord/get/${this.userId}`).then(response=>{
                this.list = response.data.data.list;
            })
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
