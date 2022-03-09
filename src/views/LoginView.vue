<template>
  <div class="container">
    <form class="row justify-content-center mt-5" @submit.prevent="signIn">
      <div class="col-6">
        <h1 class="h3">請先登入</h1>
        <div class="my-3">
          <label for="InputEmail" class="form-label text-secondary"
            >Email address</label
          >
          <input
            type="email"
            class="form-control"
            id="InputEmail"
            placeholder="Email address"
            v-model="user.username"
            autofocus
            required
          />
        </div>
        <div class="mb-3">
          <label for="InputPassword" class="form-label text-secondary"
            >Password</label
          >
          <input
            type="password"
            class="form-control"
            placeholder="Password"
            id="InputPassword"
            v-model="user.password"
            required
          />
        </div>
        <div class="text-end mt-3">
          <button type="submit" class="btn btn-lg btn-primary">登入</button>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      user: {}, //對應v-model取得帳號、密碼
    };
  },
  methods: {
    signIn() {
      const url = `${process.env.VUE_APP_API}admin/signin`;
      this.$http
        .post(url, this.user)
        .then((res) => {
          const { token,expired } = res.data; // 登入成功後取得token、expired
          document.cookie = `myToken=${token}; expires=${new Date(expired)}`;// 存取token、expired到cookie
          this.$router.push("/admin/products"); // 將頁面跳轉至後台產品列表
        })
        .catch((err) => {
          alert(err.data.message);
        });
    },
  },
};
</script>