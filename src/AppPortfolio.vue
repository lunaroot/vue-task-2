<template>
  <form class="card card-w30" @submit.prevent="submitForm">
    <div class="form-control">
      <label for="type">Тип блока</label>
      <select id="type" v-model="blockType">
        <option value="title">Заголовок</option>
        <option value="subtitle">Подзаголовок</option>
        <option value="avatar">Аватар</option>
        <option value="text">Текст</option>
      </select>
    </div>

    <div class="form-control">
      <label for="value">Значение</label>
      <textarea id="value" rows="3" v-model="blockText"></textarea>
      <div class="alert danger" v-if="error">{{ error }}</div>
    </div>

    <button class="btn primary">Добавить</button>
  </form>

  <div class="card card-w70">
    <div class="alert primary" v-if="portfolio.title && portfolio.updatedAt">Обновлено последний раз: {{ prettyDate }}</div>

    <app-portfolio-title :title="portfolio.title"></app-portfolio-title>
    <app-portfolio-avatar :avatar="portfolio.avatar"></app-portfolio-avatar>
    <app-portfolio-blocks :blocks="portfolio.blocks"></app-portfolio-blocks>

    <h3 v-if="!portfolio.title">Добавьте первый блок, чтобы увидеть результат</h3>
  </div>
</template>

<script>
import AppPortfolioTitle from './AppPortfolioTitle'
import AppPortfolioAvatar from './AppPortfolioAvatar'
import AppPortfolioBlocks from './AppPortfolioBlocks'

export default {
  props: ['portfolio'],

  emits: {
    'add-block'(opts) {
      return opts.blockText.length > 3
    }
  },

  data() {
    return {
      error: null,
      blockType: 'title',
      blockText: ''
    }
  },

  computed: {
    prettyDate() {
      return new Date(this.portfolio.updatedAt).toLocaleDateString('ru')
    }
  },

  methods: {
    isValidForm() {
      if (this.blockText.length > 3) {
        this.error = null
        return true
      }
      this.error = 'Введите текст больше 3 символов'
      return false
    },

    submitForm() {
      if (this.isValidForm()) {
        this.$emit('add-block', { blockType: this.blockType, blockText: this.blockText })
      }
      this.blockText = ''
    }
  },

  components: {
    AppPortfolioTitle,
    AppPortfolioAvatar,
    AppPortfolioBlocks
  }
}
</script>

<style scoped>
</style>
