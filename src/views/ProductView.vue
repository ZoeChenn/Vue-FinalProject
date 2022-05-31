<template>
  <div class="product container row g-0 px-md-3 mx-auto" style="">
    <div class="product-photos col-md-6 row g-0 align-self-start p-3 p-md-0">
      <div class="photo-lg">
          <img
        class="col-11"
        :src="product.imageUrl"
        alt=""
      />
      </div>
      <div class="photo-sm col-4" role="button">
        <span
          style="#"
        ></span>
      </div>
      <div class="photo-sm col-4 active" role="button">
        <span
          style="#"
        ></span>
      </div>
      <div class="photo-sm col-4" role="button">
        <span
          style="#"
        ></span>
      </div>
    </div>
    <div class="product-content col-md-6 text-center text-md-start">
      <span class="path d-block mb-4 fs-7"
        ><a href="/" class="router-link-active">首頁</a> &nbsp;/&nbsp;
        <a href="#/products" class="">產品列表</a>
        &nbsp;/&nbsp; <span class="active">麵</span></span
      >
      <h1>{{ product.title }}</h1>
      <p class="product-desc col-md-8">
        {{ product.description }}
      </p>
      <p class="text-secondary m-0">重量：300g ± 5%</p>
      <p class="product-price mt-4">
        <span>$ {{ product.price }}</span>
        <small><del>$ {{ product.origin_price }}</del></small>
      </p>
      <div class="input-group w-50 mb-3">
        <div class="product-add-qty col-9 d-flex">
          <button type="button" class="btn btn-outline-dark material-icons" disabled="">remove</button>
          <input type="text" class="text-center" value="1" max="50" min="1">
          <button type="button" class="btn btn-outline-dark material-icons">add</button>
          <button
          class="btn btn-primary col-12 col-md-7 ms-md-2 p-3 mt-3 mt-md-0"
          type="button"
        >
          加入購物車
          <div class="loading bounceball d-none fade">
            <span></span><span></span><span></span>
          </div>
        </button>
        </div>
      </div>
    </div>
    <div class="product-page row g-0 justify-content-between py-4 px-3 px-md-0">
      <button
        class="col-6 col-md-2 btn btn-outline-secondary fs-7"
        type="button"
      >
        <i class="material-icons fs-6">west</i>雙色堅果餅乾</button
      ><button
        class="col-6 col-md-2 btn btn-outline-secondary fs-7 border-start-0 d-md-none"
        type="button"
      >
        歐式麵包<i class="material-icons fs-6">east</i></button
      ><button
        class="col-6 col-md-2 btn btn-outline-secondary fs-7 d-none d-md-flex"
        type="button"
      >
        歐式麵包<i class="material-icons fs-6">east</i>
      </button>
    </div>
  </div>

  <router-view></router-view>
</template>

<script>
export default {
  data () {
    return {
      product: {}
    }
  },
  methods: {
    getProduct () {
      console.log(this.$route)
      const { id } = this.$route.params
      this.$http(
        `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/product/${id}`
      ).then((res) => {
        this.product = res.data.product
      })
    }
  },
  mounted () {
    this.getProduct()
  }
}
</script>
