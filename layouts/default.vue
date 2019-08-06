<template>
  <v-app dark>

    <v-navigation-drawer
      v-model="drawer"
      :mini-variant="miniVariant"
      :clipped="clipped"
      fixed
      app
    >
      <!-- 一番左のリンクベタベタ貼ってるデバッグ用バー -->
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
    </v-navigation-drawer>
    
    <v-app-bar
      
      fixed
      app
    >
      <!-- <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-btn
        icon
        @click.stop="miniVariant = !miniVariant"
      >
        <v-icon>mdi-{{ `chevron-${miniVariant ? 'right' : 'left'}` }}</v-icon>
      </v-btn> -->

      <!-- <v-btn
        icon
        @click.stop="clipped = !clipped"
      >
        <v-icon>mdi-application</v-icon>
      </v-btn> -->

      <!-- <v-btn
        icon
        @click.stop="fixed = !fixed"
      >
        <v-icon>mdi-minus</v-icon>
      </v-btn> -->


      <!-- <v-toolbar-title v-text="title" /> -->

      <!-- スペース生成(これ以降は右詰め) -->
      <!-- <v-spacer /> -->
      
      <!-- <v-btn
        icon
        @click.stop="rightDrawer = !rightDrawer"
      >
        <v-icon>mdi-menu</v-icon>
      </v-btn> -->
      <div class="center">
        <SearchForm/>
      </div>

      <v-spacer />

      <button class="btn-square-soft" v-on:click="MoveForUpload()">ノートを投稿</button>      

    </v-app-bar>

    <v-content>
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
    MoveForUpload(){
      window.location.href = window.location.protocol + "/UploadPage"
    }
  },
  data () {
    return {
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
