<template>
  <div v-if="blocks && blocks.length">
    <div v-for="block in blocks" :key="block.id">
      <h2>{{ block.title }}</h2>
      <p>{{ block.text }}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    blocks: {
      type: Array
    }
  }
}
</script>

<style scoped>
</style>
