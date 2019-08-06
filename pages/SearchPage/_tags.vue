<template>
    <v-container>
        <v-layout>
            <v-flex xs12>
                <div v-if="display">
                    <div class="note" v-for='thumbnail in postImage' v-bind:key='thumbnail.id'>
                        <div class="oneCard" v-if="thumbnail.pages.length != 0" >
                             <v-card>
                                <v-card-text>{{ thumbnail.subject_name }}</v-card-text>
                                <a v-on:click="loadDetail(thumbnail.id)">
                                    <v-img
                                        :src="thumbnail.pages[0].image"
                                        contain
                                        height="200"
                                    ></v-img>
                                </a>
                            </v-card>
                        </div>
                    </div>
                </div>
                <div v-else class="loader"></div>
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
      window.location.href = window.location.protocol + "//" + window.location.host + "/DetailPage/" + id;
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
  }

  .oneCard{
    height:  200px;
    width:  200px;
    margin: 35px 10px;
  }


  .loader {
    font-size: 10px;
    margin: 50px auto;
    text-indent: -9999em;
    width: 11em;
    height: 11em;
    border-radius: 50%;
    background: #ffffff;
    background: -moz-linear-gradient(left, #ffffff 10%, rgba(255,255,255, 0) 42%);
    background: -webkit-linear-gradient(left, #ffffff 10%, rgba(255,255,255, 0) 42%);
    background: -o-linear-gradient(left, #ffffff 10%, rgba(255,255,255, 0) 42%);
    background: -ms-linear-gradient(left, #ffffff 10%, rgba(255,255,255, 0) 42%);
    background: linear-gradient(to right, #ffffff 10%, rgba(255,255,255, 0) 42%);
    position: relative;
    -webkit-animation: load3 1.4s infinite linear;
    animation: load3 1.4s infinite linear;
    -webkit-transform: translateZ(0);
    -ms-transform: translateZ(0);
    transform: translateZ(0);
  }
  .loader:before {
    width: 50%;
    height: 50%;
    background: #ffffff;
    border-radius: 100% 0 0 0;
    position: absolute;
    top: 0;
    left: 0;
    content: '';
  }
  .loader:after {
    background: #303030;
    width: 75%;
    height: 75%;
    border-radius: 50%;
    content: '';
    margin: auto;
    position: absolute;
    top: 0;
    left: 0;
    bottom: 0;
    right: 0;
  }
  @-webkit-keyframes load3 {
    0% {
      -webkit-transform: rotate(0deg);
      transform: rotate(0deg);
    }
    100% {
      -webkit-transform: rotate(360deg);
      transform: rotate(360deg);
    }
  }
  @keyframes load3 {
    0% {
      -webkit-transform: rotate(0deg);
      transform: rotate(0deg);
    }
    100% {
      -webkit-transform: rotate(360deg);
      transform: rotate(360deg);
    }
  }
</style>
