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
    <p v-if="portfolio.title && portfolio.updatedAt">Обновлено последний раз: {{ prettyDate }}</p>
    <h1 v-if="portfolio.title">{{ portfolio.title }}</h1>
    <div class="avatar" v-if="portfolio.avatar">
      <img :src="portfolio.avatar" alt="avatar">
    </div>
    <div v-if="portfolio.blocks && portfolio.blocks.length">
      <div v-for="block in portfolio.blocks" :key="block.id">
        <h2>{{ block.title }}</h2>
        <p>{{ block.text }}</p>
      </div>
    </div>
    <h3 v-if="!portfolio.title">Добавьте первый блок, чтобы увидеть результат</h3>
  </div>
</template>

<script>
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
