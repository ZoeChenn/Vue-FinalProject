<template>
  <div class="container">
    <!-- 容器 -->
    <div class="row row-cols-1 row-cols-lg-3 g-5">
      <!-- 隔線系統，決定內層的數量 -->
      <!-- 響應式：前桌上，後行動 -->
      <!-- 內層不定義寬度 -->
      <div class="col" v-for="product in products" :key="product.id">
        <!-- 定義寬度 -->
        <div class="card border-light h-100 text-center" style="">
          <!-- 卡片元件 -->
          <router-link :to="`/product/${product.id}`" class="img-fluid">
            <img :src="product.imageUrl" class="card-img-top" alt="..." />
          </router-link>
          <div class="card-body">
            <h5 class="card-title">{{ product.title }}</h5>
            <p class="card-text">
              <span>$ {{ product.price }}</span>
              <small
                ><del>$ {{ product.origin_price }}</del></small
              >
            </p>
            <b-button
              type="button"
              class="btn btn-danger"
              id="liveToastBtn"
              @click="addToCart(product.id)"
              :disabled="isLoadingItem === product.id"
            >
              <span
                class="spinner-border spinner-border-sm"
                v-show="isLoadingItem === product.id"
              ></span>
              加到購物車
            </b-button>
<div class="position-fixed top-0 end-0 p-3" style="z-index: 11">
  <div id="liveToast" class="toast" role="alert" aria-live="assertive" aria-atomic="true">
    <div class="toast-header">
      <img src="" class="rounded me-2" alt="">
      <strong class="me-auto">Bootstrap</strong>
      <small>11 mins ago</small>
      <button type="button" class="btn-close" data-bs-dismiss="toast" aria-label="Close"></button>
    </div>
    <div class="toast-body">
      Hello, world! This is a toast message.
    </div>
  </div>
</div>
          </div>
        </div>
      </div>
    </div>
  </div>
  <router-view></router-view>
</template>

<script>
export default {
  data () {
    return {
      products: [],
      isLoadingItem: ''
    }
  },
  methods: {
    getProducts () {
      this.$http(
        `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/products/all`
      ).then((res) => {
        this.products = res.data.products
      })
    },
    addToCart (id, qty = 1) {
      const data = {
        product_id: id,
        qty
      }
      this.isLoadingItem = id
      this.$http
        .post(
          `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/cart`,
          { data }
        )
        .then((res) => {
          this.isLoadingItem = ''
        })
    }
  },
  mounted () {
    this.getProducts()
  }
}
</script>
