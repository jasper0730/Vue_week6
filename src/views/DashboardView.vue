<template>
  <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
    <div class="container-fluid">
      <router-link class="navbar-brand" to="/">回前台</router-link>
      <button
        class="navbar-toggler"
        type="button"
        data-bs-toggle="collapse"
        data-bs-target="#navbarSupportedContent"
        aria-controls="navbarSupportedContent"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav me-auto mb-2 mb-lg-0">
          <li class="nav-item">
            <router-link class="nav-link" to="/admin/products"
              >後台產品列表</router-link
            >
          </li>
          <li class="nav-item">
            <router-link class="nav-link" to="/admin/coupon"
              >後台優惠券</router-link
            >
          </li>
          <li class="nav-item">
            <a href="#" class="nav-link" @click.prevent="signOut">登出</a>
          </li>
        </ul>
      </div>
    </div>
  </nav>
  <router-view v-if="checkSuccess"></router-view>
</template>

<script>
export default {
  data() {
    return {
      checkSuccess: false,
    };
  },
  mounted() {
    this.checkLogin(); // 進入頁面即驗證
  },
  methods: {
    signOut() {
      document.cookie = "myToken=;expires=;"; // 清空token
      alert("登出成功");
      this.$router.push("/login"); // 回到登入畫面
    },
    checkLogin() {
      // 取出token
      const token = document.cookie.replace(
        /(?:(?:^|.*;\s*)myToken\s*=\s*([^;]*).*$)|^.*$/,
        "$1"
      );
      // 如果有token
      if (token) {
        // 預設為每次帶入Authorization
        this.$http.defaults.headers.common.Authorization = token; 
        const url = `${process.env.VUE_APP_API}api/user/check`;
        this.$http
          .post(url, { api_token: this.token })
          .then(() => {
            this.checkSuccess = true; // router-view開關
          })
          .catch((err) => {
            alert(err.data.message);
            this.$router.push("/login");
          });
      }else{
        alert("登入失敗")
        this.$router.push("/login");
      }
    },
  },
};
</script>