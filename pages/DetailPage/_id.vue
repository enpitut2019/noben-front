<template>
  <v-container>
    <h1><center>{{postImage.subject_name}}</center></h1>
    <!--div class="image" v-for='page in postImage.pages' v-bind:key='page.id'>
      <v-img
        :src="page.image"
      ></v-img>
      <br>
    </div-->

    <!--h2>このノートについているタグ</h2-->
    <hr>
    <center>
    タグ：
    <div class="tag" v-for='tag in postImage.tags'>
      <button v-on:click="searchUrl(tag.name)">{{tag.name}}</button>
    </div></center><br><br>

    <div class="pgft">
      <center>
      <button class="btn" v-on:click="beforePage">前のページ</button>
      <b v-if="maxPageNum != 0">{{nowPageNum + 1}} / {{maxPageNum}}</b>
      <b v-else>{{nowPageNum}} / {{maxPageNum}}</b>
      <button class="btn" v-on:click="nextPage">次のページ</button>
      </center>
    </div>

    <div class="image">
      <br>
      <v-img 
        :src='nowPage.image'
      ></v-img>
      <br>
    </div>

    <div class="pgft">
      <center>
      <button class="btn" v-on:click="beforePage">前のページ</button>
      <b v-if="maxPageNum != 0">{{nowPageNum + 1}} / {{maxPageNum}}</b>
      <b v-else>{{nowPageNum}} / {{maxPageNum}}</b>
      <button class="btn" v-on:click="nextPage">次のページ</button>
      </center>
    </div>

    <!--h2>このノートについているタグ</h2>
    <hr>
    <div class="tag" v-for='tag in postImage.tags'>
      <button v-on:click="searchUrl(tag.name)">{{tag.name}}</button>
    </div><br--><br>

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
      display: false,
      nowPage: 'https://haniwaman.com/wp-content/uploads/2018/01/loading-840x600.png',
      nowPageNum: 0,
      maxPageNum: 0,
      name: ""
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
    },
    searchUrl(tag){
        //if(this.content != "" && this.content.indexOf(' ') ===  -1 && this.content.indexOf('　') === -1){
            window.location.href = window.location.protocol + "/SearchPage/" + tag;
        //}else{
          //  alert("タグが不正な値です")
        //}
    },
    beforePage(){
      if (this.nowPageNum - 1 >= 0) {
        this.nowPageNum = this.nowPageNum - 1;
      }
      this.nowPage = this.postImage.pages[this.nowPageNum];
    },
    nextPage(){
      if (this.nowPageNum + 1 < this.maxPageNum) {
        this.nowPageNum = this.nowPageNum + 1;
      }
      this.nowPage = this.postImage.pages[this.nowPageNum];
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
          //if (this.postImage.pages.length != 0) {
            this.nowPage = this.postImage.pages[this.nowPageNum];
            this.maxPageNum = this.postImage.pages.length;
          /*} else {
            this.nowPageNum = -1;
          }*/
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
    h1 {
      font-size: 300%;
    }

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
        background-color: gray;
        color: white;
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

    .image {
      margin: auto;
      width: 60%;
    }

    .image v-img {
      text-align: center;
      width: 100%;
    }

    .tag {
      display: inline-block;
      margin: 3px;
      margin-top: 5px;
      background-color: white;
      color: black;
      border:none;
      border-radius:10px;
      box-shadow: none;
      padding: 2px 8px;
      position: center;
    }

    #name {
      width: 300px;
      margin-top: 5px;
      margin-bottom: 5px;
    }
</style>
