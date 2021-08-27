<template>
  <div>
    <!-- <pic-zoom :previewImg="smallImg" :zoomImg="bigImg"></pic-zoom> -->
    <pic-zoom
      :width="300"
      :height="300"
      :scale="3"
      minIMGsrc="https://renren-auction.oss-cn-beijing.aliyuncs.com/shop-test/59ded626N7c4cb0a3.jpg"
    ></pic-zoom>
  </div>
</template>
<script>
import PicZoom from "~/components/commons/PicZoom";
export default {
  components: {
    PicZoom,
  },
  data() {
    return {
      data: {
        imgUrl: "",
      },
    };
  },
};
</script>
