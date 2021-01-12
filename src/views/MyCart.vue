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
    <tr v-for="pc in shoppingcartlist" :key="pc.id">
      <td>
        {{pc.nameproduct}}
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
</template>

<script>
import axios from "axios";
import {URL_API_REST_PRODUCT_CARTS, URL_API_REST_PRODUCTS} from "@/store/constants";

export default {
  name: "MyCart",
  data() {
    return {
      productcart: null,
      shoppingcart: {
        nameproduct: null,
        sku: null,
        quantity: null,
        id: null
      },
      shoppingcartlist:[],
    }
  },
  mounted() {
    this.getCartList();
  },
  methods: {
    getCartList() {
      axios.get(URL_API_REST_PRODUCT_CARTS)
          .then(response => {
            console.log(response)
            this.productcart = response.data
            this.productcart.forEach(item => {
              this.shoppingcart.quantity = item.quantity
              this.shoppingcart.id = item.id
              axios.get(URL_API_REST_PRODUCTS + "/" + item.product_id)
              .then(response => {
                this.shoppingcart.nameproduct = response.data.name
                this.shoppingcart.sku = response.data.sku
                //var shop = this.shoppingcart
                console.log(this.shoppingcart.nameproduct)
                console.log(this.shoppingcart.sku)
                console.log(this.shoppingcart.quantity)

                this.shoppingcartlist.push(this.shoppingcart)
              })
              .catch(e => console.log(e))
            })
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
    },
    getShop(product) {
      var shop = product
      return shop
    }
  }
}
</script>
