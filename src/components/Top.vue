<template>
  <nav class="navbar navbar-expand-lg navbar-light bg-light">
    <div class="container-fluid">
      <img src="../assets/cartlogo.png" style="max-width: 50px">
      <router-link class="navbar-brand" :to="{name: 'home'}">
        MyShoppingCart
      </router-link>
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav me-auto mb-2 mb-lg-0">
          <li class="nav-item">
            <router-link class="nav-link" :to="{name: 'products'}">
              <img src="../assets/buylogo.png" style="max-width: 30px">
              Products
            </router-link>
          </li>
          <li class="nav-item">
            <router-link class="nav-link" :to="{name: 'mycart'}">
              <img src="../assets/carttab.png" style="max-width: 30px">
              My Cart
            </router-link>
          </li>
        </ul>
      </div>
    </div>
  </nav>
</template>

<script>
export default {
name: "Top"
}
</script>
