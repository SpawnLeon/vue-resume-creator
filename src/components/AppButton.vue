<template>
  <button class="btn" @click="$emit('action')"
          :class="color">
    <slot></slot>
  </button>

</template>

<script>
export default {
  emits: {
    action: null,
  },
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
