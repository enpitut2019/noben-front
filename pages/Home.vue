<template>
  <div id="home">
    <div>
      <h2>テストページ</h2>
    </div>
    <div>
      <SearchForm/>
    </div>
  </div>
</template>

<script>
import Comment from '~/components/Comment.vue'
import SearchForm from '~/components/SearchForm.vue'

export default {
  components: {
    Comment,
    SearchForm
  }
}
</script>