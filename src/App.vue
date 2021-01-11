<template>
  <div class="container column">
    <app-settings></app-settings>
    <app-portfolio></app-portfolio>
  </div>
  <app-comments></app-comments>
</template>

<script>
import AppSettings from './AppSettings'
import AppPortfolio from './AppPortfolio'
import AppComments from './AppComments'

export default {
  components: {
    AppSettings,
    AppPortfolio,
    AppComments
  }
}
</script>

<style scoped>
</style>
