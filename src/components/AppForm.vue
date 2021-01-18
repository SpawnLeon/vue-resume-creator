<template>
  <form class="card card-w30"
        @submit.prevent="creatingFormHandler">
    <div class="form-control">
      <label for="type">Тип блока</label>
      <select id="type"
              v-model="form.type">
        <option value="title">Заголовок</option>
        <option value="subtitle">Подзаголовок</option>
        <option value="avatar">Аватар</option>
        <option value="text">Текст</option>
      </select>
    </div>

    <div class="form-control">
      <label for="value">Значение</label>
      <textarea id="value"
                v-model="form.value"
                rows="3"></textarea>
    </div>
    <app-button color="primary">Добавить</app-button>
  </form>
</template>

<script>
import AppButton from './AppButton.vue';

export default {
  name: 'AppForm',
  emits: {
    createBlock: null,
  },
  components: { AppButton },
  data() {
    return {
      form: {
        type: '',
        value: '',
      },
    };
  },
  methods: {
    creatingFormHandler() {
      const { type, value } = this.form;
      if (type && value) {
        this.$emit('createBlock', { type, value });
        this.form.type = null;
        this.form.value = '';
      }
    },
  },
};
</script>

<style scoped>

</style>
