<template>
<v-container>
  <v-layout text-center wrap>

    <div id="app">
      <input type="file" @change="onFileChanged">
      <button @click="onUpload">Upload</button>
      <div v-for="image in images">
      <h2>{{image.name}}</h2>
      <img v-show="image" :src="image.thumbnail" />
      </div>
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
      images: []
    }
  },
  methods: {
    onFileChanged(event) {
      let files = event.target.files;
      this.sFile = files
      this.createImage(files[0])
    },
    onUpload() {
      const formData = new FormData()
      formData.append('note[subject_name]', 'hoge')
      for (  var i = 0;  i < this.images.length;  i++  ) {
        formData.append(`note[pages_attributes][${i}][image]`, this.images[i].file, this.images[i].name)
        formData.append(`note[pages_attributes][${i}][order]`, 0)
      }
      formData.append('tags', 'aaa, bbb, ccc')
      axios.post('https://noben.herokuapp.com/notes', formData)
    },
    createImage(file) {
      const reader = new FileReader();
      let obj = {};
      reader.onload = e => {
        obj.thumbnail = e.target.result;
        obj.file = file
        obj.name = file.name;
        this.images.push(obj);
      };
      reader.readAsDataURL(file);
    },
    remove() {
      this.sFile = false;
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
