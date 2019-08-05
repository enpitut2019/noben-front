<template>
  <v-container>
    <div>
      <getImage/>
    </div>
    <div v-for='page in postImage.pages' v-bind:key='page.id'>
      <!-- <a v-on:click="loadDetail(id)"> -->
      <v-img
        :src="page.image"
        class="my-3"
        contain
        height="200"
      ></v-img>
      <!-- </a> -->
    </div>
  </v-container>
</template>

<script>
import getImage from '~/components/getImage.vue'
import axios from 'axios'

export default {
  components: {
    getImage,
  },
  data() {
    return {
      postImage: 'https://haniwaman.com/wp-content/uploads/2018/01/loading-840x600.png'
    };
  },
  methods: {

  },
  async created() {
    // alert(this.$route.params.id)
    // this.id = this.$route.params.id
    try {
        await axios.get("https://noben.herokuapp.com/notes/" + this.$route.params.id)
        .then((res) => {
          alert(res.data.pages[0].image)
          this.postImage = res.data;
        })
        .catch( (e) =>{
          console.log(e);
        });
    } catch (e) {
      console.log(e);
    }
  }
};
</script>
