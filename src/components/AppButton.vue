<template>
  <button class="btn"
          :class="color">
    <slot></slot>
  </button>

</template>

<script>
export default {
  props: {
    color: {
      type: String,
    },

  },
  name: 'AppButton',
};
</script>

<style scoped>

</style>
