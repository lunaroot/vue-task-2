<template>
  <div class="container column">
    <app-settings></app-settings>
    <app-portfolio></app-portfolio>
  </div>
  <app-comments></app-comments>
</template>

<script>
import axios from 'axios'
import AppSettings from './AppSettings'
import AppPortfolio from './AppPortfolio'
import AppComments from './AppComments'

const DB_URL = process.env.VUE_APP_DB_URL

export default {
  async mounted() {
    await this.init()
  },

  methods: {
    async init() {
      try {
        const { data } = await axios.get(`${DB_URL}portfolio.json`)
        if (!data) {
          const now = Date.now()
          await axios.post(`${DB_URL}portfolio.json`, {
            createdAt: now,
            updatedAt: now
          })
        }
      } catch (error) {
        console.error('[init error]', error.message, Date.now())
      }
    }
  },

  components: {
    AppSettings,
    AppPortfolio,
    AppComments
  }
}
</script>

<style scoped>
</style>
