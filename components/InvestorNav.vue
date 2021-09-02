<template>
  <ul>
    <li :class="{ 'pleft-cur': $route.fullPath === '/user' }">
      <span>
        <NuxtLink to="/user" style="text-align:center;width:115px;"
          ><i class="el-icon-wallet dot1"></i>账户总览</NuxtLink
        >
      </span>
    </li>
    <li :class="{ 'pleft-cur': $route.fullPath === '/user/info' }">
      <span>
        <NuxtLink to="/user/info" style="text-align:center;width:115px;">
          <i class="el-icon-reading dot1"></i>个人信息
        </NuxtLink>
      </span>
    </li>
    <li :class="{ 'pleft-cur': $route.fullPath === '/user/fund' }">
      <span>
        <NuxtLink to="/user/fund" style="text-align:center;width:115px;">
          <i class="el-icon-coin dot1"></i>资金记录
        </NuxtLink>
      </span>
    </li>
    <li :class="{ 'pleft-cur': $route.fullPath === '/user/buyRecord' }">
      <span>
        <NuxtLink style="text-align:center;width:115px;" to="/user/buyRecord">
          <i class="el-icon-shopping-cart-2 dot1"></i>购买记录
        </NuxtLink>
      </span>
    </li>
    <li :class="{ 'pleft-cur': $route.fullPath === '/user/seller' }">
      <span>
        <NuxtLink style="text-align:center;width:115px;" to="/user/seller">
          <i class="el-icon-place dot1"></i>成为卖家
        </NuxtLink>
      </span>
    </li>
    <li
      :class="{ 'pleft-cur': $route.fullPath === '/user/auctionDetail' }"
      style="display:none"
    >
      <span>
        <NuxtLink
          style="text-align:center;width:115px;"
          to="/user/auctionDetail"
        >
          <i class="el-icon-place dot1"></i>拍品详情
        </NuxtLink>
      </span>
    </li>
    <li :class="{ 'pleft-cur': $route.fullPath === '/user/add' }">
      <span>
        <NuxtLink style="text-align:center;width:115px;" to="/user/add">
          <i class="el-icon-place dot1"></i>添加拍品
        </NuxtLink>
      </span>
    </li>
    <li :class="{ 'pleft-cur': $route.fullPath === '/user/auctionList' }">
      <span>
        <NuxtLink style="text-align:center;width:115px;" to="/user/auctionList">
          <i class="el-icon-place dot1"></i>拍卖列表
        </NuxtLink>
      </span>
    </li>
    <li :class="{ 'pleft-cur': $route.fullPath === '/user/offer' }">
      <span>
        <NuxtLink style="text-align:center;width:115px;" to="/user/offer">
          <i class="el-icon-place dot1"></i>出价记录
        </NuxtLink>
      </span>
    </li>
    <li :class="{ 'pleft-cur': $route.fullPath === '/user/recharge' }">
      <span
        ><NuxtLink to="/user/recharge"
          ><i class="el-icon-thumb dot03"></i>充值</NuxtLink
        ></span
      >
    </li>
    <li :class="{ 'pleft-cur': $route.fullPath === '/user/withdraw' }">
      <span
        ><NuxtLink to="/user/withdraw"
          ><i class="el-icon-bank-card dot04"></i>提现</NuxtLink
        ></span
      >
    </li>
  </ul>
</template>

<script>
export default {};
</script>
