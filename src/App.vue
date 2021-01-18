<template>
  <div class="container column">
    <app-form @createBlock="createBlock" />
    <app-card class-name="card-w70">
      <div v-if="blocks.length">
        <div
          class="list__item item-list"
          v-for="block in blocks"
          :key="block"
        >
          <component
            :is="`app-${getBlockType(block)}`"
            :value="getBlockValue(block)"
          ></component>

          <app-button color="danger item-list__close"
                      @action="removeBlock(block.id)">&times;
          </app-button>
        </div>

      </div>
      <h3 v-else>Добавьте пер блок, чтобы увидеть результат</h3>
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
      blocks: [],
      comments: [],
      isCommentsLoading: false,
    };
  },
  methods: {
    async getComments() {
      this.isCommentsLoading = true;
      const response = await fetch('https://jsonplaceholder.typicode.com/comments?_limit=15', {
        method: 'get',
        headers: { 'Content-Type': 'application/json' },
      });
      const comments = await response.json();
      this.isCommentsLoading = false;
      return comments;
    },

    async onClickLoadComments() {
      this.comments = await this.getComments();
    },

    getBlockType: (block) => block.type,

    getBlockValue: (block) => block.value,

    async createBlock({ type, value }) {
      const newBLock = { value, type };
      const res = await fetch(
        'https://vue-resume-creator-default-rtdb.europe-west1.firebasedatabase.app/block.json', {
          method: 'post',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify(newBLock),
        },
      );

      const data = await res.json();
      newBLock.id = data.name;
      this.blocks.push(newBLock);
    },

    async removeBlock(id) {
      await fetch(
        `https://vue-resume-creator-default-rtdb.europe-west1.firebasedatabase.app/block/${id}.json`, {
          method: 'delete',
          headers: { 'Content-Type': 'application/json' },
        },
      );
      this.blocks = this.blocks.filter((el) => el.id !== id);
    },

    async loadBlocks() {
      let blocks = [];
      const response = await fetch(
        'https://vue-resume-creator-default-rtdb.europe-west1.firebasedatabase.app/block.json', {
          method: 'get',
          headers: { 'Content-Type': 'application/json' },
        },
      );
      const data = await response.json();

      blocks = Object.keys(data).map((key) => ({
        id: key,
        ...data[key],
      }));

      return blocks;
    },
  },
  async mounted() {
    const blocks = await this.loadBlocks();
    this.blocks = blocks;
  },

};
</script>

<style lang="scss">
.avatar {
  display         : flex;
  justify-content : center;
}

.avatar img {
  width         : 150px;
  height        : auto;
  border-radius : 50%;
}

.list {
  // .list__item
  &__item {
    display         : flex;
    align-items     : center;
    justify-content : space-between;
    position        : relative;
    padding-right   : 30px;

    > * {
      flex-grow : 1;
    }
  }
}

.item-list {
  // .item-list__close
  &__close {
    position : absolute;
    top      : 0px;
    right    : 0px;
    padding  : 5px;
    height   : 30px;
    width    : 30px;
  }
}

</style>
