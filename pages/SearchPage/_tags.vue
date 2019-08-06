<template>
    <v-container>
        <v-layout>
            <v-flex xs12>
                <div v-if="display">
                    <div class="note" v-for='thumbnail in postImage' v-bind:key='thumbnail.id'>
                        <div v-if="thumbnail.pages.length != 0" >
                            <a v-on:click="loadDetail(thumbnail.id)">
                                <v-img
                                    :src="thumbnail.pages[0].image"
                                    contain
                                    height="200"
                                ></v-img>
                            </a>
                        </div>
                    </div>
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
      display: false,
    };
  },
  methods: {
    loadDetail(id){
      window.location.href = window.location.protocol + "/DetailPage/" + id
    }
  },
  async created() {
    try {
        await axios.get("https://noben.herokuapp.com/notes?search_words=" + this.$route.params.tags)
        .then((res) => {
          this.postImage = res.data;
          this.display = true;
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
    vertical-align: top;
    height:  200px;
    width:  200px;
    margin: 10px;
  }
</style>
