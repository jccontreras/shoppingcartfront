<template>
  <div class="container-fluid">
    <form @submit.prevent="editCart">
      <div class="col">
        <div class="card" style="width: 18rem;">
          <div class="card-body">
            <h5 class="card-title">{{ product.name }}</h5>
            <p class="card-text">ID: {{ product.product_id }}</p>
            <p class="card-text">Sku: {{ product.sku }}</p>
            <p class="card-text">Description: {{ product.description }}</p>
          </div>
          <div class="card-body">
            <input type="number" class="count" name="qty" value="1" min="1" style="max-width: 100px"
                   v-model="product.quantity">
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
  props: {
    cartItem: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      product: {
        name: null,
        id: null,
        sku: null,
        quantity: null,
        product_id: null,
        cart_id: null,
        description: null,
      },
      product_cart: {
        product_id: null,
        cart_id: null,
        quantity: null,
      }
    }
  },
  methods: {
    editCart() {
      this.product_cart.product_id = this.product.product_id
      this.product_cart.cart_id = this.product.cart_id
      this.product_cart.quantity = this.product.quantity
      this.product_cart.id = this.product.id
      //axios.put(`${constants.URL_API_REST_PRODUCT_CARTS}`, this.product_cart)
      axios.put(constants.URL_API_REST_PRODUCT_CARTS, this.product_cart)
        .then(() => {
          this.$emit('updated');
        })
        .catch()
    },
  },
  mounted() {
    this.product = this.cartItem;
  },
}
</script>
