<template>
  <v-container>
    <div>
      <getImage/>
    </div>
    <!-- <div v-for='(page, i) in pages' :key='postImage.id'> -->
      {{id}}
    <!-- </div> -->
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
      postImage: 'https://haniwaman.com/wp-content/uploads/2018/01/loading-840x600.png',
      id: ""
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
