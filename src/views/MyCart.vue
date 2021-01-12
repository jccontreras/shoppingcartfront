<template>
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
    <tr v-for="pc in productcart" :key="pc.id">
      <td>Mark</td>
      <td>
        {{ pc.quantity }}
      </td>
      <td>@mdo</td>
        <td>
          <button type="button" class="btn btn btn-light" @click="deleteProduct(pc)">
            <img src="../assets/deletelogo.png" style="max-width: 30px">
          </button>
        </td>
    </tr>
    </tbody>
  </table>
</template>

<script>
import axios from "axios";
import {URL_API_REST_PRODUCT_CARTS} from "@/store/constants";

export default {
  name: "MyCart",
  data() {
    return {
      productcart: null,
      shoppingcart: {
        nameproduct: null,
        sku: null,
        quantity: null
      },
    }
  },
  mounted() {
    this.getCartList();
  },
  methods: {
    getCartList() {
      axios.get(URL_API_REST_PRODUCT_CARTS)
          .then(response => {
            this.productcart = response.data
          })
          .catch(e => console.log(e))
    },
    deleteProduct(idprod) {
      console.log(idprod.id)
      var url = URL_API_REST_PRODUCT_CARTS + "/" + idprod.id;
      axios.delete(url)
          .then(response => {
            console.log(response)
            this.getCartList()
          })
          .catch(e => console.log(e))
    }
  }
}
</script>
