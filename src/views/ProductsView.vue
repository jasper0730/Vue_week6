<template>
  <div class="container py-3">
    <Loading :active="isLoading"></Loading>
    <h1>產品列表</h1>
    <table class="table align-middle">
      <thead>
        <tr>
          <th>圖片</th>
          <th>商品名稱</th>
          <th>價格</th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="product in products" :key="product.id">
          <td>
            <img :src="product.imageUrl" alt="..." />
          </td>
          <td>{{ product.title }}</td>
          <td>
            <del class="h6 text-secondary">原價{{ product.origin_price }}</del>
            <p class="h5">現在只要{{ product.price }}</p>
          </td>
          <td>
            <div class="btn-group">
              <button
                type="button"
                class="btn btn-outline-secondary me-1 rounded"
                @click="getProduct(product.id)"
              >
                查看更多
              </button>
              <button
                type="button"
                class="btn btn-outline-danger rounded"
                @click="addToCart(product.id)"
              >
                加入購物車
              </button>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
    <ProductModal 
    ref="productModal" 
    :product="product"
    @add-to-cart="addToCart"
    ></ProductModal>
  </div>
</template>

<script>
import ProductModal from "@/components/ProductModal.vue";
export default {
  data() {
    return {
      products: [],
      product: {},
      isLoading: false,
    };
  },
  methods: {
    // 加入購物車
    addToCart(id, qty = 1) {
      this.isLoading = true;
      const url = `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/cart`;
      const data = {
        data: {
          product_id: id,
          qty,
        },
      };
      this.$http
        .post(url, data)
        .then((res) => {
          this.isLoading = false;
          alert(res.data.message);
          this.$refs.productModal.hideModal();
        })
        .catch((err) => {
          alert(err.data.message);
        });
    },
    // 產品列表
    getProducts() {
      this.isLoading = true;
      this.$http
        .get(
          `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/products/all`
        )
        .then((res) => {
          this.products = res.data.products;
          this.isLoading = false;
        })
        .catch((err) => {
          alert(err.data.message);
        });
    },
    // 單一產品
    getProduct(id) {
      this.isLoading = true;
      this.product = {};
      this.$http
        .get(
          `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/product/${id}`
        )
        .then((res) => {
          this.product = res.data.product;
          this.isLoading = false;
          this.$refs.productModal.openModal();
        })
        .catch((err) => {
          alert(err.data.message);
        });
    },
  },
  components: {
    ProductModal,
  },
  mounted() {
    this.getProducts();
  },
};
</script>