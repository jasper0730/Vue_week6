<template>
<!-- 將導覽列拆成獨立元件 -->
  <FrontNavbar></FrontNavbar>
  <router-view></router-view>
</template>

<script>
// 載入導覽列元件
import FrontNavbar from "@/components/FrontNavbar.vue";

export default {
  components: {
    FrontNavbar,
  },
};
</script>
