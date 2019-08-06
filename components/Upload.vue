<template>
<v-container>
  <v-layout text-center wrap>

    <div id="app">
      <input type="file" @change="onFileChanged">
      <button @click="onUpload">Upload</button>
      <img v-show="sFile" :src="sFile" />
      <!-- <form @submit.prevent="upload" enctype="multipart/form-data">
        <input @change="selectedFile" type="file" name="file" ref="file">
        <img v-show="uploadFile" :src="uploadFile" />
        <button type="submit">アップロード</button>

        <div v-show="uploadFile" class="preview-item-btn" @click="remove">
          <button class="preview-item-icon">close</button>
        </div>

      </form> -->
    </div>



  </v-layout>
</v-container>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      //uploadFile: null,
      sFile: null
    }
  },
  methods: {
    onFileChanged(event) {
      this.sFile = event.target.files[0]
      let files = event.target.files;
      this.createImage(files[0])
    },
    onUpload() {
      const formData = new FormData()
      formData.append('note[subject_name]', 'hoge')
      formData.append('note[pages_attributes][0][image]', this.sFile, this.sFile.name)
      formData.append('note[pages_attributes][0][order]', 0)
      formData.append('note[pages_attributes][1][image]', this.sFile, this.sFile.name)
      formData.append('note[pages_attributes][1][order]', 0)
      formData.append('tags', 'aaa, bbb, ccc')
      axios.post('https://noben.herokuapp.com/notes', formData)
    },
    createImage(file) {
      const reader = new FileReader();
      reader.onload = e => {
        this.sFile = e.target.result;
      };
      reader.readAsDataURL(file);
    },
    remove() {
      this.uploadFile = false;
    },
    // selectedFile: function(e) {
    //   // 選択された File の情報を保存しておく
    //   e.preventDefault();
    //   this.uploadFile = this.$refs.file.files[0];
    //   this.createImage(this.uploadFile);
    // },
    // upload: function() {
    //   // FormData を利用して File を POST する
    //   let formData = new FormData();
    //   formData.append('note[subject_name]', "hogee");
    //   formData.append('note[pages_attributes][0][image]', this.uploadFile);
    //   formData.append('note[pages_attributes][0][order]', "0");
    //   let config = {
    //     headers: {
    //       'Content-Type': 'multipart/form-data'
    //     }
    //   };
    //   axios
    //     .post('https://noben.herokuapp.com/notes', formData)
    //     .then(function(response) {
    //       // response 処理
    //     })
    //     .catch(function(error) {
    //       // error 処理
    //     })
    // },
  }

};
</script>
