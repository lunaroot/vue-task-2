<template>
  <div class="container column">
    <app-portfolio :portfolio="portfolio"></app-portfolio>
  </div>
  <app-comments></app-comments>
</template>

<script>
import axios from 'axios'
import AppPortfolio from './AppPortfolio'
import AppComments from './AppComments'

const DB_URL = process.env.VUE_APP_DB_URL

export default {
  data() {
    return {
      portfolio: null
    }
  },

  async mounted() {
    await this.loadPortfolio()
  },

  methods: {
    async loadPortfolio() {
      try {
        const { data } = await axios.get(`${DB_URL}portfolio.json`)
        if (data) {
          this.portfolio = data
        } else {
          const now = Date.now()
          const portfolio = {
            createdAt: now,
            updatedAt: now
          }
          await axios.post(`${DB_URL}portfolio.json`, portfolio)
          this.portfolio = portfolio
        }
      } catch (error) {
        console.error('[load portfolio error]', error.message, Date.now())
      }
    }
  },

  components: {
    AppPortfolio,
    AppComments
  }
}
</script>

<style scoped>
</style>
