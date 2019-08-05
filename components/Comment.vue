<template>
    <div class="cmt">
        <input v-model="content" type="text" name="content" placeholder="ノートにコメントをしよう！"><button class="btn"
         v-on:click="postComment">コメント</button>
    </div>
</template>

<script>
    import axios from 'axios';
    export default {
        // メソッドで使う&テンプレート内で使う変数を定義
        data() {
            return {
                //title:'',
                content:''
            }
        },
        // (読み込み時に)実行するメソッド
        methods: {
            postComment(){
                var comment = {
                    //'title': this.title,
                    'content': this.content
                };
                var ID = 1;
                axios.post('https://noben.herokuapp.com/notes/' + ID + '/comments', comment).then(res => {
                    //console.log(res.data.title);
                    console.log(res.data.content);
                });
            }

        }
    }
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