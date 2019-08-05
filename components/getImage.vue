<template>
  <v-container>
    <v-layout
      text-center
      wrap
    >
    
    <div class="notes">
    <v-flex xs12>
        <v-img
          :src="postImage"
          class="my-3"
          contain
          height="200"
        ></v-img>
    </v-flex>
    </div>
     
    </v-layout>
  </v-container>
</template>

<script>
import axios from 'axios';
export default {
  data() {
    return {
      postImage: 'https://haniwaman.com/wp-content/uploads/2018/01/loading-840x600.png',
      URL: "",
    };
  },
  methods: {
  },
  async created() {
    try {
        await axios.get("https://noben.herokuapp.com/notes")
        .then((res) => {
          this.postImage = res.data[0].image_url;
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

<style>
  .notes {
    display: inline-block;
    transform: scale(2, 2);
  }
</style>