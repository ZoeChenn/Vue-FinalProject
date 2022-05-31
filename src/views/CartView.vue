
<template>
  <div class="main container">
    <div class="d-none d-md-flex row justify-content-center my-5">
      <div class="col-md- col-lg-7">
        <div class="d-flex justify-content-between">
          <div class="d-flex flex-column align-items-center">
            <div
              class="border border-2 rounded-circle p-1 p-md-2 mb-3 bg-primary"
            ></div>
            <span class="text-center">確認購物車</span>
          </div>
          <div class="progressLine"></div>
          <div class="d-flex flex-column align-items-center">
            <div class="border border-2 rounded-circle p-1 p-md-2 mb-3"></div>
            <span class="text-center">訂購資訊</span>
          </div>
          <div class="progressLine"></div>
          <div class="d-flex flex-column align-items-center">
            <div class="border border-2 rounded-circle p-1 p-md-2 mb-3"></div>
            <span class="text-center">確認結帳</span>
          </div>
          <div class="progressLine"></div>
          <div class="d-flex flex-column align-items-center">
            <div class="border border-2 rounded-circle p-1 p-md-2 mb-3"></div>
            <span class="text-center">訂購成功</span>
          </div>
        </div>
      </div>
    </div>
    <div class="d-md-none row g-0 mb-3">
      <div class="col-6">
        <div class="fontSizeXS border text-center py-1 progressFocus">
          確認購物車
        </div>
      </div>
      <div class="col-6">
        <div class="fontSizeXS border text-center py-1">訂購資訊</div>
      </div>
      <div class="col-6">
        <div class="fontSizeXS border text-center py-1">確認結帳</div>
      </div>
      <div class="col-6">
        <div class="fontSizeXS border text-center py-1">訂購成功</div>
      </div>
    </div>
    <h2 class="fontSizeM fontSize-md-L text-center mb-5">購物車</h2>
    <p>總共有<span class="fw-bold mx-1">{{ cartData.length }}</span>件商品</p>
    <table class="table align-middle d-none d-md-table mb-5">
      <thead>
        <tr class="text-center">
          <th></th>
          <th width="200">品名</th>
          <th></th>
          <th width="90">數量</th>
          <th>單價</th>
          <th>小計</th>
        </tr>
      </thead>
      <tbody>
        <template v-if="cartData.carts">
        <tr class="text-center" v-for="item in cartData.carts" :key="item.id">
          <th scope="row">
          </th>
          <td>
            <div
              :style="{ backgroundImage: `url(${item.product.imageUrl})` }"
              style="
                height: 100px;
                background-size: cover;
                background-position: center;
              "
            ></div>
          </td>
          <td>{{ item.product.title }}</td>
          <td>
            <div class="input-group">
              <select id="" class="form-select" v-model="item.qty" @change="updateCartItem(item)" :disabled="isLoadingItem === item.id"><option :value="num" v-for="num in 20" :key="`${num}-${item.id}`">{{ num }}</option></select>
            </div>
          </td>
          <td>{{ item.product.price }}</td>
          <td>{{ item.product.price*item.qty }}</td>
          <td>
            <button type="button" class="btn" @click="removeCartItem(item.id)">
              <span class="material-icons">delete_outline</span>
            </button>
          </td>
        </tr>
        </template>
      </tbody>
    </table>
    <div class="d-flex mb-6 mb-md-7">
      <!----><button type="button" class="btn btn-outline-secondary" @click="removeAllCartItems" :disabled="cartData.total === 0">
        刪除全部商品
      </button>
    </div>
    <div class="row justify-content-end mb-6 mb-md-7">
      <div class="col-md-5">
        <div class="d-flex justify-content-between mb-2">
          <p class="mb-0">折扣前總金額:</p>
          <span>{{ cartData.total }}</span>
        </div>
        <div class="d-flex justify-content-between align-items-center mb-2">
          <button
            type="button"
            class="btn btn-warning btn-sm"
            data-bs-toggle="modal"
            data-bs-target="#showCoupons"
          >
            選擇優惠券</button
          ><input
            type="text"
            id="couponCode"
            placeholder="請選擇優惠券"
            class="form-control w-50"
            aria-describedby="couponCode"
            disabled=""
          />
        </div>
        <!---->
      </div>
    </div>
    <div class="d-flex justify-content-center mb-6">
      <a href="#/products" class="btn btn-outline-secondary me-3" role="button">
        繼續購物 </a
      ><a href="#/buyerForm" class="btn btn-secondary me-3" role="button">
        填寫訂購資訊
      </a>
    </div>
  </div>

  <router-view></router-view>
</template>

<script>
// import emitter from '@/libs/emitter'

export default {
  data () {
    return {
      cartData: {},
      products: [],
      isLoadingItem: ''
    }
  },
  methods: {
    getProducts () {
      this.$http
        .get(
          `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/products/all`
        )
        .then((res) => {
          console.log('Product列表：', res)
          this.products = res.data.products
        })
    },
    getCart () {
      this.$http.get(`${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/cart`)
        .then((res) => {
          console.log('Cart列表：', res)
          this.cartData = res.data.data
        })
    },
    removeCartItem (id) {
      this.$http.delete(`${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/cart/${id}`)
        .then((res) => {
          this.getCart()
        })
    },
    removeAllCartItems () {
      this.$http.delete(`${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/carts`)
        .then((res) => {
          this.getCart()
        })
    },
    updateCartItem (item) {
      const data = {
        product_id: item.id,
        qty: item.qty
      }
      this.isLoadingItem = item.id
      this.$http.put(`${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/cart/${item.id}`, { data })
        .then((res) => {
          this.getCart()
          this.isLoadingItem = ''
        })
    }
  },
  mounted () {
    this.getProducts()
    this.getCart()
  }
}
</script>
