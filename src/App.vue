<template>
  <div id="app" class="container-fluid">
    <top/>
    <router-view/>
  </div>
</template>

<script>
import Top from './components/Top.vue'
import axios from "axios";
import {URL_API_REST_CARTS} from "@/store/constants";

export default {
  name: 'App',
  components: {
    Top
  },
  data() {
    return {
      carts: null,
      cart: {
        status: "pending",
      },
    }
  },
  mounted() {
    this.addCart();
  },
  //This method creat a new cart when you go into page
  methods: {
    addCart() {
      axios.post(URL_API_REST_CARTS, this.cart)
      .then(response => {
        console.log(response)
        this.carts = response.data
        console.log("response data " + this.carts)
      })
      .catch(e => {
        console.log(e)
      })
    },
  }
}
</script>

