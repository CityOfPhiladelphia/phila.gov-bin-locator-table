<template>
  <div id="vue-app-1">
    <BinLocator />
  </div>
</template>

<script>
import BinLocator from './components/BinLocator.vue'

export default {
  name: 'App',
  components: {
    BinLocator,
  }
}
</script>

<style>
/* #vue-app-1 {
  width: 70rem;
  margin: 0 auto;
} */

thead tr th {
    width: 50%;
  }
</style>
