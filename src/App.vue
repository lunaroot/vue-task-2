<template>
  <div class="container column">
    <div class="alert danger" v-if="error">
      Возникла ошибка при загрузке: {{ error }}
    </div>
    <div class="loader" v-if="loading"></div>
    <app-portfolio :portfolio="portfolio" @add-block="addBlock" v-if="!error && !loading"></app-portfolio>
  </div>
  <app-comments></app-comments>
</template>

<script>
import axios from 'axios'
import { v4 as uuid } from 'uuid'
import AppPortfolio from './AppPortfolio'
import AppComments from './AppComments'

const DB_URL = process.env.VUE_APP_DB_URL

export default {
  data() {
    return {
      error: null,
      loading: false,
      portfolio: {}
    }
  },

  async mounted() {
    await this.loadPortfolio()
  },

  methods: {
    async loadPortfolio() {
      try {
        this.error = null
        this.loading = true
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
        this.loading = false
      } catch (error) {
        console.error('[load portfolio error]', error.message, Date.now())
        this.error = error.message
        this.loading = false
      }
    },

    async addBlock(opts) {
      const currentBlocks = this.portfolio.blocks || []

      if (opts.blockType === 'title') {
        this.portfolio.title = opts.blockText
      }

      if (opts.blockType === 'avatar') {
        this.portfolio.avatar = opts.blockText
      }

      if (opts.blockType === 'subtitle') {
        const block = currentBlocks.find((b) => !b.title)
        if (block) {
          block.title = opts.blockText
          this.portfolio.blocks = currentBlocks.map((b) => {
            if (b.id === block.id) {
              b.title = block.title
            }
            return b
          })
        } else {
          const newBlock = [{ id: uuid(), title: opts.blockText }]
          this.portfolio.blocks = [ ...newBlock, ...currentBlocks ]
        }
      }

      if (opts.blockType === 'text') {
        const block = currentBlocks.find((b) => !b.text)
        if (block) {
          block.text = opts.blockText
          this.portfolio.blocks = currentBlocks.map((b) => {
            if (b.id === block.id) {
              b.text = block.text
            }
            return b
          })
        } else {
          const newBlock = [{ id: uuid(), text: opts.blockText }]
          this.portfolio.blocks = [ ...newBlock, ...currentBlocks ]
        }
      }

      this.portfolio.updatedAt = Date.now()
      const portfolioID = this.portfolio.id
      if (!portfolioID) {
        throw new Error('portfolioID required')
      }

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
