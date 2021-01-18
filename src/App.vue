<template>
  <div class="container column">
    <app-form @createBlock="createBlock" />
    <app-card class-name="card-w70">
      <div v-if="blocks.length">
        <component
          :is="`app-${getBlockType(block)}`"
          v-for="block in blocks"
          :key="block"
          :value="getBlockValue(block)"></component>
      </div>
      <h3 v-else>Добавьте первый блок, чтобы увидеть результат</h3>
    </app-card>
  </div>
  <div class="container">
    <p>
      <app-button @action="onClickLoadComments"
                  color="primary">Загрузить комментарии
      </app-button>
    </p>
    <app-card v-if="comments.length">
      <app-comment-list :comments="comments" />
    </app-card>
    <app-loader v-if="isCommentsLoading" />
  </div>
</template>

<script>
import axios from 'axios';
import AppLoader from './components/AppLoader.vue';
import AppForm from './components/AppForm.vue';
import AppButton from './components/AppButton.vue';
import AppCommentList from './components/AppCommentList.vue';
import AppTitle from './components/AppTitle.vue';
import AppSubtitle from './components/AppSubtitle.vue';
import AppAvatar from './components/AppAvatar.vue';
import AppText from './components/AppText.vue';
import AppCard from './components/AppCard.vue';

export default {
  components: {
    AppTitle,
    AppCommentList,
    AppButton,
    AppLoader,
    AppForm,
    AppSubtitle,
    AppAvatar,
    AppText,
    AppCard,

  },
  data() {
    return {
      blocks: [
        {
          id: 1,
          value: 'Резюме Nickname',
          // title | subtitle | avatar | text
          type: 'title',
        },
        {
          id: 2,
          value: 'Опыт работы',
          type: 'subtitle',
        },
        {
          id: 3,
          value: 'https://cdn.dribbble.com/users/5592443/screenshots/14279501/drbl_pop_r_m_rick_4x.png',
          type: 'avatar',
        },

        {
          id: 4,
          value: `Главный герой американского мультсериала «Рик и Морти», гениальный учёный, изобретатель,
          атеист (хотя в некоторых сериях он даже молится Богу, однако, каждый раз после чудесного
          спасения ссылается на удачу и вновь отвергает его существование),
          алкоголик, социопат, дедушка Морти. На момент начала третьего сезона ему 70 лет[1]`,
          type: 'text',
        },
      ],
      comments: [],
      isCommentsLoading: false,
    };
  },
  methods: {
    async getComments() {
      this.isCommentsLoading = true;
      const response = await axios.get('https://jsonplaceholder.typicode.com/comments?_limit=15');
      const { data } = response;
      this.isCommentsLoading = false;
      return data;
    },
    async onClickLoadComments() {
      this.comments = await this.getComments();
    },
    getBlockType: (block) => block.type,
    getBlockValue: (block) => block.value,
    createBlock({ type, value }) {
      const newBLock = { id: Math.random(), value, type };
      this.blocks.push(newBLock);
    },
  },

};
</script>

<style>
.avatar {
  display         : flex;
  justify-content : center;
}

.avatar img {
  width         : 150px;
  height        : auto;
  border-radius : 50%;
}
</style>
