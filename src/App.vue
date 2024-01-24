<template>
  <AppHeader />
  <main class="container">
    <router-view></router-view>
  </main>
</template>

<script>
import AppHeader from './components/AppHeader.vue';
export default {
  name: "App",
  data() {
    return {

    }
  },
  components: {
    AppHeader
  },
  methods: {

  },

  mounted() {

  }
}
</script>

<style lang="scss" scoped>
@use "./assets/styles/partials/variables.scss" as *;

h2 {
  color: $brand_primary;
}
</style>