<template>
  <div class="container-fluid">
    <form @submit.prevent="editCart(this.product)">
      <div class="col">
        <div class="card" style="width: 18rem;">
          <div class="card-body">
            <h5 class="card-title">{{ this.product.name }}</h5>
            <p class="card-text">ID: {{ this.product.id }}</p>
            <p class="card-text">Sku: {{ this.product.sku }}</p>
            <p class="card-text">Description: {{ this.product.description }}</p>
          </div>
          <div class="card-body">
            <input type="number" class="count" name="qty" value="1" min="1" style="max-width: 100px"
                   v-model="this.product_cart.quantity">
          </div>
          <div class="card-body">
            <button type="submit" class="btn btn-primary" id="editButton">
              Ready
            </button>
          </div>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
import * as constants from '@/store/constants'
import axios from "axios";

export default {
  name: "EditProductCart",
  data() {
    return {
      product_cart: {},
      product: {},
    }
  },
  mounted() {
    this.getProduct();
  },
  methods: {
    getProduct(idP) {
      axios.get(constants.URL_API_REST_PRODUCTS + "/" +idP)
      .then(({data}) => {
        this.product.id = data.id
        this.product.name = data.name
        this.product.sku = data.sku
        this.description = data.description
      })
      .catch((e) => console.log(e))
    }
  },
}
</script>
