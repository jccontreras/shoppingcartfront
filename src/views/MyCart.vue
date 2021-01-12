<template>
  <div class="container-fluid">
    <div class="row">
      <table class="table">
        <thead>
        <tr>
          <th scope="col">Product Name</th>
          <th scope="col">#</th>
          <th scope="col">SKU</th>
          <th scope="col">Actions</th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="pc in shoppingcartlist" :key="pc.id">
          <td>
            {{ pc.nameproduct }}
          </td>
          <td>
            {{ pc.quantity }}
          </td>
          <td>
            {{ pc.sku }}
          </td>
          <td>
            <button type="button" class="btn btn btn-light" @click="deleteProduct(pc)">
              <img src="../assets/deletelogo.png" style="max-width: 30px">
            </button>
          </td>
        </tr>
        </tbody>
      </table>
      <div class="alert alert-success" role="alert" v-if="changestatus" style="max-width: 400px">
        <label>The cart has been completed.</label>
      </div>
      <form @submit.prevent="changeStatus()">
        <div class="col-2">
          <div class="input-group input-group-sm mb-3">
            <span class="input-group-text" id="inputGroup-sizing-sm">Cart Status</span>
            <input type="text" class="form-control" aria-label="Sizing example input"
                   aria-describedby="inputGroup-sizing-sm" v-model="this.cart.status" disabled>
          </div>
        </div>
        <div class="col-2">
          <button type="submit" class="btn btn-warning">Checkout</button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import * as constants from "@/store/constants";

export default {
  name: "MyCart",
  data() {
    return {
      statuscarts: false,
      changestatus: false,
      productcart: null,
      shoppingcartlist: [],
      cart: {}
    }
  },
  mounted() {
    this.getCartList();
    this.getCartStatus(1);
  },
  watch: {
    changestatus(val) {
      setTimeout(() => {
        if (val) this.changestatus = false;
      }, 3000);
    },
  },
  methods: {
    getCartList() {
      this.shoppingcartlist = []
      axios.get(constants.URL_API_REST_PRODUCT_CARTS)
          .then(({data}) => {
            this.productcart = data
            this.productcart.forEach(item => {
              var shop = {}
              shop.quantity = item.quantity
              shop.id = item.id
              axios.get(constants.URL_API_REST_PRODUCTS + "/" + item.product_id)
                  .then(({data}) => {
                    shop.nameproduct = data.name
                    shop.sku = data.sku
                    this.shoppingcartlist.push(shop)
                  })
                  .catch(e => console.log(e))
            })
          })
          .catch(e => console.log(e))
    },
    getCartStatus(cartId) {
      this.cart = {}
      axios.get(constants.URL_API_REST_CARTS + "/" + cartId)
          .then(({data}) => {
            this.cart.status = data.status
            this.cart.id = cartId
          })
          .catch((e) => console.log(e))
    },
    deleteProduct(idprod) {
      console.log(idprod.id)
      var url = constants.URL_API_REST_PRODUCT_CARTS + "/" + idprod.id;
      axios.delete(url)
          .then(response => {
            console.log(response)
            this.getCartList()
          })
          .catch(e => console.log(e))
    },
    changeStatus() {
      console.log("Entro a completed")
      this.cart.status = "completed"
      axios.put(constants.URL_API_REST_CARTS, this.cart)
          .then(({data}) => {
            console.log(data)
            this.changestatus = true
            this.getCartStatus(1)
          })
          .catch((e) => console.log(e))
    }
  }
}
</script>
