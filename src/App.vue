<template>
  <div id="app" class="container-fluid">
    <top/>
    <router-view/>
  </div>
</template>

<script>
import Top from './components/Top.vue'

export default {
  name: 'App',
  components: {
    Top
  }
}
</script>

