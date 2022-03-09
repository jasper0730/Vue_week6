<template>
  <div class="about">
    <Loading :active="isLoading"></Loading>
    <div class="container">
      <h1 class="my-3">購物車</h1>
      <div class="row justify-content-center">
        <div class="col-md-6">
          <table class="table align-middle">
            <thead>
              <tr>
                <th style="width: 110px"></th>
                <th>品名</th>
                <th style="width: 110px">數量</th>
                <th>單價</th>
              </tr>
            </thead>
            <tbody>
              <template v-if="cartsData.carts">
                <tr v-for="item in cartsData.carts" :key="item.id">
                  <td>
                    <button
                      type="button"
                      class="btn btn-outline-danger btn-sm"
                      @click="deleteProduct(item.id)"
                    >
                      移除品項
                    </button>
                  </td>
                  <td>
                    {{ item.product.title }}
                    <div v-if="item.coupon" class="text-success">
                      已套用優惠券
                    </div>
                  </td>
                  <td>
                    <div class="input-group">
                      {{ item.qty }}/{{ item.product.unit }}
                    </div>
                  </td>
                  <td class="text-end">
                    <small>折扣價</small>{{ item.final_total }}
                  </td>
                </tr>
              </template>
            </tbody>
          </table>
        </div>
      </div>
      <div class="row justify-content-center">
        <Form 
        class="col-md-6 g-3" 
        @submit="createOrder" 
        ref="form"
        v-slot="{ errors }">
          <div class="mb-3">
            <label for="email" class="form-label">Email</label>
            <Field
              type="text"
              name="email"
              class="form-control"
              id="email"
              placeholder="請輸入Email"
              rules="email|required"
              :class="{ 'is-invalid': errors['email'] }"
              v-model="form.data.user.email"
            />
            <ErrorMessage name="email" class="invalid-feedback"></ErrorMessage>
          </div>
          <div class="mb-3">
            <label for="name" class="form-label">訂購人姓名</label>
            <Field
              type="text"
              name="姓名"
              class="form-control"
              id="name"
              placeholder="請輸入姓名"
              rules="required"
              :class="{ 'is-invalid': errors['姓名'] }"
              v-model="form.data.user.name"
            />
            <ErrorMessage name="姓名" class="invalid-feedback"></ErrorMessage>
          </div>
          <div>
            <div class="mb-3">
              <label for="tel" class="form-label">訂購人電話</label>
              <Field
                class="form-control"
                type="text"
                name="電話"
                id="tel"
                placeholder="請輸入電話"
                rules="required"
                :class="{ 'is-invalid': errors['電話'] }"
                v-model="form.data.user.tel"
              />
              <ErrorMessage name="電話" class="invalid-feedback"></ErrorMessage>
            </div>
            <div class="mb-3">
              <label for="address" class="form-label">訂購人地址</label>
              <Field
                class="form-control"
                type="text"
                name="地址"
                id="address"
                placeholder="請輸入地址"
                rules="required"
                :class="{ 'is-invalid': errors['地址'] }"
                v-model="form.data.user.address"
              />
              <ErrorMessage name="地址" class="invalid-feedback"></ErrorMessage>
            </div>
            <div class="mb-3">
              <label for="message" class="form-label">備註</label>
              <textarea
                class="form-control"
                type="text"
                id="message"
                cols="30"
                rows="10"
                v-model="form.data.message"
              >
              </textarea>
            </div>
            <div class="text-end">
              <button type="submit" class="btn btn-danger mb-3">送出</button>
            </div>
          </div>
        </Form>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isLoading: false,
      cartsData: {},
      form: {
        data: {
          user: {
            name: "",
            email: "",
            tel: "",
            address: "",
          },
          message: "",
        },
      },
    };
  },
  methods: {
    getCartsData() {
      this.isLoading = true; // 打開loading動態
      const url = `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/cart`;
      this.$http.get(url).then((res) => {
        // 如果購物車友資料success為true
        if (res.data.success) {
          this.cartsData = res.data.data;
          this.isLoading = false; // 關閉loading動態
        } else {
          alert(res.data.message);
        }
      });
    },
    deleteProduct(id) {
      this.isLoading = true;
      const url = `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/cart/${id}`;
      this.$http
        .delete(url)
        .then((res) => {
          this.getCartsData();
          alert(res.data.message);
          this.isLoading = true;
        })
        .catch((err) => {
          alert(err.data.message);
        });
    },
    createOrder() {
      this.isLoading = true;
      const url = `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/order`;
      this.$http
        .post(url, this.form)
        .then((res) => {
          alert(res.data.message);
          this.$refs.form.resetForm(); // 清空表單資料
          this.isLoading = false;
          this.getCartsData(); // 重新取得購物車資料
        })
        .catch((err) => {
          alert(err.data.message);
        });
    },
  },
  mounted() {
    this.getCartsData();
  },
};
</script>