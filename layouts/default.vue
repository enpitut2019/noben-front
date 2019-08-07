<template>
  <v-app dark>

    <!-- 一番左のリンクベタベタ貼ってるデバッグ用バー -->
    <!-- <v-navigation-drawer
      v-model="drawer"
      :mini-variant="miniVariant"
      :clipped="clipped"
      fixed
      app
      >
      <v-list>
        <v-list-item
          v-for="(item, i) in items"
          :key="i"
          :to="item.to"
          router
          exact
          >
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title v-text="item.title" />
          </v-list-item-content>

        </v-list-item>
      </v-list>
    </v-navigation-drawer> -->

    <v-app-bar fixed>
      <v-btn depressed @click="MoveForTop">
        NoBen
      </v-btn>

      <v-spacer></v-spacer>

      <v-text-field
        v-model="content"
        hide-details
        >
      </v-text-field>
      <v-btn
        icon >
        <v-icon
          @click="MoveForSearch">
          mdi-magnify</v-icon>
      </v-btn>

      <v-spacer></v-spacer>

      <v-btn
        @click="MoveForUpload"
        icon>
        <v-icon>mdi-plus</v-icon>
      </v-btn>
    </v-app-bar>

    <v-content class="mt-12">
      <v-container>
        <nuxt />
      </v-container>
    </v-content>

    <!-- <v-navigation-drawer
      v-model="rightDrawer"
      :right="right"
      temporary
      fixed
      >
      <v-list>
      <v-list-item @click.native="right = !right">
      <v-list-item-action>
      <v-icon light>
      mdi-repeat
      </v-icon>
      </v-list-item-action>
      <v-list-item-title>Switch drawer</v-list-item-title>
      </v-list-item>
      </v-list>
      </v-navigation-drawer> -->

      <!-- フッター -->
      <v-footer
        :fixed="fixed"
        app
        >
        <span>&copy; 2019/8/7</span>
      </v-footer>
  </v-app>
</template>

<script>
import SearchForm from '~/components/SearchForm.vue'

export default {
  components: {
    SearchForm
  },
  methods: {
    MoveForTop(){
      // alert(window.location.protocol +"\n"+window.location.host+"\n"+window.location.hostname)
      window.location.href = window.location.protocol + "//" + window.location.host;
    },
    MoveForUpload(){
      window.location.href = window.location.protocol + "//" + window.location.host + "/UploadPage";
    },
    MoveForSearch(){
      if(this.content != "" && this.content.indexOf(' ') ===  -1 && this.content.indexOf('　') === -1){
        window.location.href = window.location.protocol + "//" + window.location.host + "/SearchPage/" + this.content;
      }else{
        alert("タグが不正な値です")
      }
    },
  },
  data () {
    return {
      content: '',
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          title: 'index',
          to: '/'
        },
        {
          icon: 'mdi-apps',
          title: 'Home',
          to: '/Home'
        },
        {
          title: 'DetailPage',
          to: '/DetailPage'
        },
        {
          title: 'UploadPage',
          to: '/UploadPage'
        }
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'NoBen'
    }
  }
}
</script>


<style>
  .center{
    text-align: center;
  }

  .btn-square-soft {
    display: inline-block;
    position: relative;
    text-decoration: none;
    color: #302f2f;
    width: 120px;
    height: 50px;
    line-height: 50px;
    border-radius: 5px;
    text-align: center;
    overflow: hidden;
    font-weight: bold;
    background: linear-gradient(#c4c0c2 0%, #8d8a8a 100%);
    text-shadow: 1px 1px 1px rgba(255, 255, 255, 0.66);
    box-shadow: 0 1px 1px rgba(0, 0, 0, 0.28);
  }

  .btn-square-soft:active {
    /*押したとき*/
    -webkit-transform: translateY(2px);
    transform: translateY(2px);/*沈むように*/
    box-shadow: 0 0 1px rgba(0, 0, 0, 0.15);
    background-image: linear-gradient(#dad6d7 0%, #949191 100%);/*グラデーションを明るく*/
  }
</style>
