<template>
  <div class="container">
    <h1 class="my-3">單一產品</h1>
    {{ product }}
  </div>
</template>

<script>
export default {
  data() {
    return {
      product: {},
    };
  },
  methods: {
    getProducts() {
      const { id } = this.$route.params;
      this.$http(`${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/product/${id}`)
        .then((res) => {
        this.product = res.data.product;
      });
    },
  },
  mounted() {
    this.getProducts();
  },
};
</script>