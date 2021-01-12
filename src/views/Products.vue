<template>
  <div class="container-fluid">
    <div v-for="product in products" :key="product.id" class="accordion accordion-flush"
         >
      <div class="accordion-item">
        <h2 class="accordion-header" id="flush-headingOne">
          <button class="accordion-button collapsed" type="button" data-bs-toggle="collapse"
                  data-bs-target="#flush-collapseOne" aria-expanded="false" aria-controls="flush-collapseOne">
            {{product.name}}
          </button>
        </h2>
        <div id="flush-collapseOne" class="accordion-collapse collapse" aria-labelledby="flush-headingOne"
             data-bs-parent="#accordionFlushExample">
          <div class="accordion-body">
           Sku: {{product.sku}}
           Description: {{product.description}}
          </div>
        </div>
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
      products: null
    }
  },
  mounted() {
    this.getProducts();
  },
  methods: {
    getProducts() {
      axios.get('http://192.168.1.106:9000/api/v1/products')
          .then( response => {
            console.log(response)
            this.products = response.data
          })
      .catch(e => console.log(e))

    }
  }
}
</script>
