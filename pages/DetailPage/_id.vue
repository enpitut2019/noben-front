<template>
  <v-container>
    <h1><center><button v-on:click="searchUrl(postImage.subject_name)">{{postImage.subject_name}}</button></center></h1>
    <!--div class="image" v-for='page in postImage.pages' v-bind:key='page.id'>
      <v-img
        :src="page.image"
      ></v-img>
      <br>
    </div-->

    <!--h2>このノートについているタグ</h2-->
    <div v-if="display">
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
    </div>
    <div v-else class="loader"></div>
    
    <br>
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
        <input id="name" v-model="user_name" type="text" name="user_name" placeholder="ユーザーネーム"><br>
        <input v-model="content" type="text" name="content" placeholder="ノートにコメントをしよう！"><button class="btn"
         v-on:click="postComment()">コメント</button>
    </div>
    <br>
    <br>
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
      user_name: "",
    };
  },
  methods: {
    async postComment(){
      var comment = {
          'user_name': this.user_name,
          'content': this.content
      };
      if (comment['content'] != "") {
        await axios.post('https://noben.herokuapp.com/notes/' + this.$route.params.id + '/comments', comment).then(res => {
          console.log(res.data.user_name);
          console.log(res.data.content);
        });
        window.location.reload();
      }
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
