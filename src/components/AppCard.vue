<template>
  <div class="card"
       :class="className">
    <slot></slot>
  </div>
</template>

<script>
export default {
  props: {
    className: {
      type: String,
    },
  },
  name: 'AppCard',
};
</script>

<style scoped>

</style>
