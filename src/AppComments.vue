<template>
  <div class="container">
    <div class="card" v-if="comments.length">
      <h2>Комментарии ({{ comments.length }})</h2>
      <ul class="list">
        <li class="list-item" v-for="comment in comments" :key="comment.id">
          <div>
            <p><strong>{{ comment.email }}</strong></p>
            <small>{{ comment.body }}</small>
          </div>
        </li>
      </ul>
    </div>
    <p v-else>
      <button class="btn primary" @click="loadComments">Загрузить комментарии</button>
    </p>
    <div class="alert danger" v-if="error">
      Возникла ошибка при загрузке: {{ error }}
    </div>
    <div class="loader" v-if="loading"></div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      error: null,
      loading: false,
      comments: []
    }
  },

  methods: {
    async loadComments() {
      try {
        this.loading = true
        const { data } = await axios.get('https://jsonplaceholder.typicode.com/comments?_limit=42')
        this.comments = data
        this.error = null
        this.loading = false
      } catch (error) {
        this.error = error.message
        this.loading = false
      }
    }
  }
}
</script>

<style scoped>
</style>
