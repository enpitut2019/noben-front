<template>
  <v-container>
    <div v-for='page in postImage.pages' v-bind:key='page.id'>
      <v-img
        :src="page.image"
        contain
        height="200"
      ></v-img>
    </div>

    <!-- コメントを一覧表示 -->
    <div v-if="display">
      <div v-for='comment in postImage.comments' v-bind:key='comment.id'>
        {{ comment.content }}
        <!-- <div v-if="comment.length != 0" >
        </div> -->
      </div>
    </div>

    <!--  -->
    <div class="cmt">
        <input v-model="content" type="text" name="content" placeholder="タグを入力しよう！"><button class="btn"
         v-on:click="postComment">検索</button>
    </div>
  </v-container>
</template>

<script>
// import Comment from '~/components/Comment.vue'
import axios from 'axios'

export default {
  components: {
    // Comment,
  },
  data() {
    return {
      content: ""
    };
  },
  methods: {
    
  },
  async created() {
    // alert(this.$route.params.id)
    try {
        await axios.get("https://noben.herokuapp.com/notes/" + this.$route.params.id)
        .then((res) => {
          // alert(res.data.pages[0].image)
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
    textarea, input {
        color: black;
        background-color: white;
        width: 80%;
        /*margin: auto;
        position: absolute;*/
        border:none;
        border-radius:10px;
        box-shadow: none;
        padding: 2px 8px;
    }

    input, button:focus {
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

    .cmt {
        width: 100%;
        margin: auto;
        position: absolute;
    }
</style>