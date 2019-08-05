<template>
  <div id="home">
    <div>
      <h2>テストページ</h2>
    </div>
    <div>
        <Comment/>
    </div>
  </div>
</template>

<script>
import Comment from '~/components/Comment.vue'

export default {
  components: {
    Comment,
  }
}
</script>