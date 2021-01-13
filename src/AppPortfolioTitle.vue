<template>
  <h1 v-if="title">{{ title }}</h1>
</template>

<script>
export default {
  props: {
    title: {
      type: String
    }
  }
}
</script>

<style scoped>
</style>
