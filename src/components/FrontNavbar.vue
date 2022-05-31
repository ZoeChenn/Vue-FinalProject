<template>
<div class="container d-flex flex-column" >
<nav class="navbar navbar-expand-lg navbar-light">
    <router-link class="navbar-brand" to="/">貓爵餐廳</router-link>
    <button
      class="navbar-toggler"
      type="button"
      data-bs-toggle="collapse"
      data-bs-target="#navbarNavAltMarkup"
      aria-controls="navbarNavAltMarkup"
      aria-expanded="false"
      aria-label="Toggle navigation"
    >
      <span class="navbar-toggler-icon"></span>
    </button>
    <div
      class="collapse navbar-collapse justify-content-end"
    >
      <div class="navbar-nav">
        <li class="nav-item">
            <router-link class="nav-item nav-link me-4 active" to="#"
              >關於<span class="sr-only"></span
            ></router-link>
          </li>
          <li class="nav-item">
            <router-link class="nav-item nav-link me-4" to="/products"
              >產品列表</router-link
            >
          </li>
          <li class="nav-item">
            <router-link
              to="/cart"
              ><font-awesome-icon :icon="['fa','cart-shopping']" class="fa-lg" /><span class="position-absolute top-50 start-100 translate-middle badge rounded-pill bg-danger">{{
          cartData.carts.length
        }}</span>
              </router-link>
          </li>
      </div>
    </div>
  </nav>
</div>
</template>

<script>
import emitter from '@/libs/emitter'

export default {
  data () {
    return {
      cartData: {
        carts: []
      }
    }
  },
  methods: {
    getCart () {
      this.$http
        .get(`${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/cart`)
        .then((res) => {
          this.cartData = res.data.data
        })
    }
  },
  mounted () {
    this.getCart()
    emitter.on('get-cart', () => {
      this.getCart()
    })
  }
}
</script>
