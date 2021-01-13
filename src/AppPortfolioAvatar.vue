<template>
  <div class="avatar" v-if="avatar">
    <img :src="avatar" alt="avatar">
  </div>
</template>

<script>
export default {
  props: {
    avatar: {
      type: String
    }
  }
}
</script>

<style scoped>
  .avatar {
    display: flex;
    justify-content: center;
  }
  .avatar img {
    width: 150px;
    height: auto;
    border-radius: 50%;
  }
</style>
