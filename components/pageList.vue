<template>
  <v-container>
    <v-layout
      
      
    >

    <v-flex xs12>
    <!-- <div v-for='(page, i) in pages' :key='postImage.id'> -->
    <div class="note" v-for="i in postImage.length">
      <a v-on:click="loadDetail(id)">
        <v-img
          :src="postImage[i].image_url"
          class="my-3"
          contain
          height="200"
        ></v-img>
      </a>
    </div>
    </v-flex>


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
      id: 1,
    };
  },
  methods: {
    loadDetail(id){
      window.location.href = "https://noben.herokuapp.com/notes/" + id
    }
  },
  async created() {
    try {
        await axios.get("https://noben.herokuapp.com/notes")
        .then((res) => {
          // alert(res.data[4].image_url);
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

<style>
  .note {
    display: inline-block;
    height:  200px;
    width:  200px;
    margin: 10px;
  }
</style>
