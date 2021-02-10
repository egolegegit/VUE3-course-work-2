/* eslint-disable quotes */
<template>
  <div class="card">
    <h4>Комментарии временно не достуны</h4>
    <p class="class-item">{{ textError1[0] }}</p>
    <div class="btn-close" @click="$emit('close-CommentsError')"></div>
  </div>
</template>

<script>
export default {
  props: ['textError1']
}
</script>

<style></style>
