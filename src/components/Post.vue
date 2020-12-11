<template>
  <div class="post">
    <div>
      <img :src="post.url" :alt="post.title"/>
    </div>
    <div>
      <h3>{{ post.title }}</h3>
    </div>
  </div>
</template>
<script>
export default {
  props: ['post']
}
</script>
<style>
  .post {
    display: flex;
    flex-direction: row;
  }
</style>
