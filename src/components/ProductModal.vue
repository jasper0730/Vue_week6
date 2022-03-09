<template>
  <div
    class="modal fade"
    id="productModal"
    tabindex="-1"
    aria-labelledby="productModalLabel"
    aria-hidden="true"
    ref="modal"
  >
    <div class="modal-dialog modal-xl">
      <div class="modal-content">
        <div class="modal-header bg-dark text-white">
          <h5 class="modal-title" id="productModalLabel">
            {{ product.title }}
          </h5>
          <button
            type="button"
            class="btn-close"
            data-bs-dismiss="modal"
            aria-label="Close"
          ></button>
        </div>
        <div class="modal-body">
          <div class="row">
            <div class="col-sm-6">
              <img class="img-fluid" :src="product.imageUrl" alt="" />
            </div>
            <div class="col-sm-6">
              <h5>
                <span class="badge mb-1 bg-primary rounded-pill">
                  {{ product.category }}
                </span>
              </h5>
              <p class="h5 mb-3">商品描述:{{ product.description }}</p>
              <p class="h5 my-3">商品內容:{{ product.content }}</p>
              <div v-if="!product.price">
                <p class="h5">{{ product.origin_price }}</p>
              </div>
              <div v-if="product.price">
                <del class="h6 text-secondary"
                  >原價{{ product.origin_price }}</del
                >
                <div class="h5 fw-bold">現在只要{{ product.price }}元</div>
              </div>

              <div class="input-group mb-3">
                <input
                  type="number"
                  class="form-control"
                  v-model.number="qty"
                  min="1"
                />
                <button
                  class="btn btn-outline-secondary"
                  type="button"
                  @click="$emit('add-to-cart', product.id, qty)"
                >
                  加入購物車
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Modal from "bootstrap/js/dist/modal";
export default {
  props: {
    product: {
      type: Object,
      default() {
        return {};
      },
    },
  },
  data() {
    return {
      modal: "",
      qty: 1,
    };
  },
  mounted() {
    this.modal = new Modal(this.$refs.modal, {
      keyboard: false,
    });
  },
  methods: {
    openModal() {
      this.qty = 1; // 沒次開啟modal之前將數字調整回1
      this.modal.show();
    },
    hideModal() {
      this.modal.hide();
    },
  },
};
</script>