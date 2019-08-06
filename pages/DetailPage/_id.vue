<template>
  <v-container>
    <div class="image" v-for='page in postImage.pages' v-bind:key='page.id'>
      <v-img
        :src="page.image"
      ></v-img>
      <br>
    </div>

    <!-- コメントを一覧表示 -->
    <h2>コメント欄</h2>
    <hr>
    <div v-if="display">
      <div v-for='(comment, i) in postImage.comments' v-bind:key='comment.id'>
          {{i+1}}. <b>{{ comment.user_name }}</b>
          <br>
          {{ comment.content }}
          <hr color="gray" size="1px;">
        <!-- <div v-if="comment.length != 0" >
        </div> -->
      </div>
    </div>

    <!--  -->
    <div class="cmt">
        <input id="name" v-model="name" type="text" name="name" placeholder="ユーザーネーム"><br>
        <input v-model="content" type="text" name="content" placeholder="ノートにコメントをしよう！"><button class="btn"
         v-on:click="postComment">コメント</button>
    </div>
    <br>
    <br>
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
      postImage: 'https://haniwaman.com/wp-content/uploads/2018/01/loading-840x600.png',
      content: "",
      display: false
    };
  },
  methods: {
    async postComment(){
      var comment = {
          'user_name': this.name,
          'content': this.content
      };
      await axios.post('https://noben.herokuapp.com/notes/' + this.$route.params.id + '/comments', comment).then(res => {
          console.log(res.data.name);
          console.log(res.data.content);
      });
      window.location.reload();
    }
  },
  async created() {
    // alert(this.$route.params.id)
    // this.id = this.$route.params.id
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
    .cmt input {
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

    .cmt input, button:focus {
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

    .box img {
      width: 100%
    }

    .image v-img {
      text-align: center;
      width: 100%;
    }

    #name {
      width: 300px;
      margin-top: 5px;
      margin-bottom: 5px;
    }
</style>