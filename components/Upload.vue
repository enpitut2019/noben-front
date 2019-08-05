<template>
  <v-container>
    <v-layout
      text-center
      wrap
    >

    <div id="app">
        <input @change="selectedFile" type="file" name="file">
        <img v-show="uploadFile" :src="uploadFile" />
        <button @click="upload" type="submit">アップロード</button>
        <div v-show="uploadFile" class="preview-item-btn" @click="remove">
          <button class="preview-item-icon">close</button>
        </div>
    </div>



    </v-layout>
  </v-container>
</template>

<script>
import axios from 'axios';
export default {
  data() {
    return {
      uploadFile: null
    }
  },
  methods: {
            selectedFile: function(e) {
                // 選択された File の情報を保存しておく
                e.preventDefault();
                let files = e.target.files;
                this.uploadFile = files[0];
                this.createImage(files[0]);
            },
            upload: function() {
                // FormData を利用して File を POST する
                let formData = new FormData();
                formData.append('note[subject_name]', "hogee");
                formData.append('note[pages_attributes][0][image]', this.uploadFile);
                //formData.append('note[pages_attributes][0][order]', "0");
                let config = {
                    headers: {
                        'content-type': 'multipart/form-data'
                    }
                };
                axios
                    .post('https://noben.herokuapp.com/notes', formData, config)
                    .then(function(response) {
                        // response 処理
                    })
                    .catch(function(error) {
                        // error 処理
                    })
            },
            createImage(file) {
              const reader = new FileReader();
              reader.onload = e => {
                this.uploadFile = e.target.result;
              };
              reader.readAsDataURL(file);
            },
            remove() {
              this.uploadFile = false;
            },
        }

};
</script>
