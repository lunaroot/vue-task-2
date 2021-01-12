<template>
  <div class="container column">
    <app-portfolio :portfolio="portfolio" @add-block="addBlock"></app-portfolio>
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
      portfolio: {}
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
          const id = Object.keys(data)[0]
          this.portfolio = { id, ...data[id] }
        } else {
          const now = Date.now()
          const portfolio = {
            createdAt: now,
            updatedAt: now
          }
          const { data } = await axios.post(`${DB_URL}portfolio.json`, portfolio)
          this.portfolio = { id: data.name, ...portfolio }
        }
      } catch (error) {
        console.error('[load portfolio error]', error.message, Date.now())
      }
    },

    async addBlock(opts) {
      if (opts.blockType === 'title') {
        this.portfolio.title = opts.blockText
      }
      if (opts.blockType === 'avatar') {
        this.portfolio.avatar = opts.blockText
      }
      this.portfolio.updatedAt = Date.now()
      const portfolioID = this.portfolio.id
      if (!portfolioID) {
        throw new Error('portfolioID required')
      }
      delete this.portfolio.id
      try {
        await axios.put(`${DB_URL}portfolio/${portfolioID}.json`, this.portfolio)
      } catch (error) {
        console.error('[add block]', error.message, Date.now())
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
