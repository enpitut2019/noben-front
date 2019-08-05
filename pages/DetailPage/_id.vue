<template>
  <v-container>
    <div>
      <getImage/>
    </div>
    {{text}}
  </v-container>
</template>

<script>
import getImage from '~/components/getImage.vue'

export default {
  components: {
    getImage,
  },
  data() {
    return {
      postImage: 'https://haniwaman.com/wp-content/uploads/2018/01/loading-840x600.png',
      text: "text"
    };
  },
  methods: {

  },
  created() {
    alert($route.params.id)
    // this.text = $route.params.id
  }
};
</script>
