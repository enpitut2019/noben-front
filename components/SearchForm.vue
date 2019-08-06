<template>
  <v-container>
      　　　
    <div class="tagSearch">
        <input v-model="content" type="text" name="content" placeholder="タグを入力" ><button class="btn"
         v-on:click="Search"><b>検索</b></button>
    </div>
    <br>
    
  </v-container>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      content: "",
    };
  },
  methods: {
    Search(){
        if(this.content != "" && this.content.indexOf(' ') ===  -1 && this.content.indexOf('　') === -1){
            window.location.href = window.location.protocol + "/SearchPage/" + this.content;
        }else{
            alert("タグが不正な値です")
        }
    }

  },
  async created() {
  }
};
</script>


<style>
    .tagSearch input {
        color: black;
        background-color: white;
        width: 40%;
        /*margin: auto;
        position: absolute;*/
        border:none;
        border-radius:10px;
        box-shadow: none;
        padding: 2px 8px;
    }

    .tagSearch input, button:focus {
        outline: none;
    }

    .btn {
        margin: 0 0 0 auto;
        margin-left: 3px;
        background-color: white;
        color: black;
        /*width: 20%;*/
        /*margin: auto;*/
        /*position: absolute;*/
        border:none;
        border-radius:10px;
        box-shadow: none;
        padding: 2px 8px;
        position: center;
    }

    .tagSearch {
        width: 100%;
        margin: auto;
        position: absolute;
    }
</style>