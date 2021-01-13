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
        <tr v-for="(pc, idx) in shoppingcartlist" :key="pc.id">
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
            <button type="button" class="btn btn btn-light" @click="editProduct(idx)">
              <img src="../assets/editlogo.png" style="max-width: 30px">
            </button>
            <button type="button" class="btn btn btn-light" @click="deleteProduct(pc)">
              <img src="../assets/deletelogo.png" style="max-width: 30px">
            </button>
          </td>
        </tr>
        </tbody>
      </table>
      <div class="alert alert-danger" role="alert" v-if="this.delete" style="max-width: 400px">
        <label>The product has been deleted from the cart</label>
      </div>
      <div class="alert alert-success" role="alert" v-if="changestatus" style="max-width: 400px">
        <label>The cart has been completed.</label>
      </div>
      <div class="col" v-if="edit">
        <a class="btn" @click="closeEdit">
          <img src="../assets/closelogo.png" style="max-width: 20px">
        </a>
        <EditProductCart :cartItem="element" @updated="getCartList"/>
      </div>
      <form @submit.prevent="changeStatus('completed')">
        <div class="col-2">
          <div class="input-group input-group-sm mb-3">
            <span class="input-group-text" id="inputGroup-sizing-sm">Cart Status</span>
            <input type="text" class="form-control" aria-label="Sizing example input"
                   aria-describedby="inputGroup-sizing-sm" v-model="cart.status" disabled>
          </div>
        </div>
        <div class="col-2">
          <button type="submit" class="btn btn-warning" :disabled="this.statuscart == 'completed'">Checkout</button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import * as constants from "@/store/constants";
import EditProductCart from '@/components/EditProductCart.vue'

export default {
  name: "MyCart",components: {
    EditProductCart,
  },
  data() {
    return {
      statuscart: "pending",
      changestatus: false,
      delete: false,
      productcart: null,
      shoppingcartlist: [],
      cart: {},
      element: {},
    }
  },
  mounted() {
    this.getCartList();
    this.getCartStatus(1);
  },
  computed: {
    edit() {
      return Object.keys(this.element).length;
    }
  },
  watch: {
    changestatus(val) {
      setTimeout(() => {
        if (val) this.changestatus = false;
      }, 3000);
    },
    delete(val) {
      setTimeout(() => {
        if (val) this.delete = false;
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
              let shop = {}
              shop.quantity = item.quantity
              shop.id = item.id
              shop.product_id = item.product_id
              shop.cart_id = item.cart_id
              axios.get(constants.URL_API_REST_PRODUCTS + "/" + item.product_id)
                  .then(({data}) => {
                    shop.nameproduct = data.name
                    shop.sku = data.sku
                    shop.description = data.description
                    this.shoppingcartlist.push(shop)
                  })
                  .catch(e => console.log(e))
            })
          })
          .catch(e => console.log(e))
          .finally(() => {
            this.element = {};
          })
    },
    getCartStatus(cartId) {
      this.cart = {}
      axios.get(constants.URL_API_REST_CARTS + "/" + cartId)
          .then(({data}) => {
            this.cart.status = data.status
            this.cart.id = cartId
            this.statuscart = this.cart.status
          })
          .catch((e) => console.log(e))
    },
    deleteProduct(idprod) {
      console.log(idprod.id)
      var url = constants.URL_API_REST_PRODUCT_CARTS + "/" + idprod.id;
      axios.delete(url)
          .then(response => {
            console.log(response)
            this.delete = true
            this.getCartList()
          })
          .catch(e => console.log(e))
    },
    changeStatus(status) {
      console.log("Entro a completed")
      this.cart.status = status
      axios.put(constants.URL_API_REST_CARTS, this.cart)
          .then(({data}) => {
            console.log(data)
            this.changestatus = true
            this.statuscart = status
            this.getCartStatus(1)
          })
          .catch((e) => console.log(e))
    },
    editProduct(idx) {
      this.element = this.shoppingcartlist[idx];
    },
    closeEdit() {
      this.element = {};
    }
  }
}
</script>
