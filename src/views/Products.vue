<template>
  <div class="container px-4">
    <div class="alert alert-danger" role="alert" v-if="error" style="max-width: 400px">
      <h4 class="alert-heading">Ups!</h4>
      <label>This has a trouble</label>
      <hr>
      <label class="mb-0">try again later please.</label>
    </div>
    <div class="alert alert-success" role="alert" v-if="add" style="max-width: 400px">
      <h4 class="alert-heading">Ready!</h4>
      <label>The product has been added to a cart</label>
    </div>
    <div class="row gx-5">
      <div v-for="product in products" :key="product.id" class="col">
        <form @submit.prevent="addToCart(product)">
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
                       v-model="quiantitylist['quant'+product.id]">
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

export default {
  name: "Products",
  data() {
    return {
      error: false,
      add: false,
      products: null,
      productcarts: {
        product_id: null,
        cart_id: null,
        quantity: 1,
      },
      quiantitylist:[],
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
      axios.get('http://192.168.1.106:9000/api/v1/products')
          .then(response => {
            console.log(response)
            this.products = response.data
          })
          .catch(e => console.log(e))
    },
    //This method add to Cart a product
    addToCart(product) {
      console.log(product)
      this.productcarts.cart_id = 1;
      this.productcarts.product_id = product.id;
      this.productcarts.quantity = this.quiantitylist['quant'+product.id]
      axios.post("http://192.168.1.106:9000/api/v1/productcarts", this.productcarts)
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
