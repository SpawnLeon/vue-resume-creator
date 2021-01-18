<template>
  <h2>Комментарии</h2>
  <ul class="list"
      v-if="comments.length">
    <li class="list-item"
        :key="comment.id"
        v-for="comment in comments">
      <div>
        <p><strong>{{ comment.email }}</strong></p>
        <small>
          {{ comment.bosy }}
        </small>
      </div>
    </li>
  </ul>
</template>

<script>
export default {
  props: {
    comments: {
      type: Array,
    },
  },
  name: 'AppCommentList',
};
</script>

<style scoped>

</style>
