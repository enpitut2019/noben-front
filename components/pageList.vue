<template>
  <v-container>
    <v-layout
      
      
    >

    <v-flex xs12>
    <div class="note" v-for="i in postImage.length">
      <v-img
        :src="postImage[i-1].image_url"
        class="my-3"
        contain
        height="200"
      ></v-img>
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
    };
  },
  methods: {
  },
  async created() {
    try {
        // await axios({
        //   method  : 'GET',
        //   url     : "https://noben.herokuapp.com/notes",
        //   headers: {
        //   'Access-Control-Allow-Origin': '*',
        //   'Content-Type': 'application/json',
        //   },}
        // )
        await axios.get("https://noben.herokuapp.com/notes")
        .then((res) => {
          // alert(res.data[4].image_url);
          this.postImage = res.data;
          // axios.get(this.URL,{crossdomain: true})
          // .then((res) => {
          //   this.postImage = res.data;
          // })
          // .catch( (e) =>{
          //   console.log(e);
          // });
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
