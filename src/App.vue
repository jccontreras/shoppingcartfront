<template>
  <div id="app" class="container-fluid">
    <top/>
    <router-view/>
  </div>
</template>

<script>
import Top from './components/Top.vue'
import axios from "axios";

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
      axios.post("http://192.168.1.106:9000/api/v1/carts", this.cart)
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

