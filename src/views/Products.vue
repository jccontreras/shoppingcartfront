<template>
  <div class="container px-4">
    <div class="alert alert-danger" role="alert" v-if="error" style="max-width: 400px">
      <label>This has a trouble</label>
      <hr>
      <label class="mb-0">try again later please.</label>
    </div>
    <div class="alert alert-success" role="alert" v-if="add" style="max-width: 400px">
      <label>The product has been added to the cart</label>
    </div>
    <div class="row gx-5">
      <div v-for="(product, idx) in products" :key="product.id" class="col">
        <form @submit.prevent="addToCart(idx)">
          <div class="p-3 border bg-light">
            <div class="card" style="width: 18rem;">
              <div class="card-body">
                <h5 class="card-title">{{ product.name }}</h5>
                <p class="card-text">ID: {{ product.id }}</p>
                <p class="card-text">Sku: {{ product.sku }}</p>
                <p class="card-text">Description: {{ product.description }}</p>
              </div>
              <div class="card-body">
                <input type="number" class="count" name="qty" value="1" min="1" style="max-width: 100px"
                       v-model="quiantitylist['quant'+product.id]" required>
              </div>
              <div class="card-body">
                <button type="submit" class="btn btn-primary" id="sendButton">
                  add to Cart
                  <img src="../assets/cartplus.png" style="max-width: 10%">
                </button>
              </div>
            </div>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import {URL_API_REST_PRODUCT_CARTS, URL_API_REST_PRODUCTS} from '../store/constants'

export default {
  name: "Products",
  data() {
    return {
      error: false,
      add: false,
      products: [],
      productcarts: {
        product_id: null,
        cart_id: null,
        quantity: 1,
      },
      quiantitylist: [],
    }
  },
  mounted() {
    this.getProducts();
  },
  watch: {
    add(val) {
      setTimeout(() => {
        if (val) this.add = false;
      }, 3000);
    },
    error(val) {
      setTimeout(() => {
        if (val) this.error = false;
      }, 3000);
    },
  },
  methods: {
    //This methos get List of products
    getProducts() {
      axios.get(URL_API_REST_PRODUCTS)
          .then(response => {
            console.log(response)
            this.products = response.data
          })
          .catch(e => console.log(e))
    },
    //This method add to Cart a product
    addToCart(idx) {
      console.log(idx)
      this.productcarts.cart_id = 1;
      this.productcarts.product_id = this.products[idx].id;
      let q = this.quiantitylist['quant' + this.products[idx].id]
      console.log("q ==> "+q)
      if (q === '0') {
        this.productcarts.quantity = '1'
      } else {
        this.productcarts.quantity = this.quiantitylist['quant' + this.products[idx].id]
      }
      axios.post(URL_API_REST_PRODUCT_CARTS, this.productcarts)
          .then(response => {
            console.log(response)
            this.add = true;
          })
          .catch(e => {
            console.log(e)
            this.error = true;
          })
    }
  }
}
</script>
