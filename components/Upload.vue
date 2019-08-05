<template>
  <v-container>
    <v-layout
      text-center
      wrap
    >
    <!-- <div class="contents">
      <label v-show="!uploadedImage" class="input-item__label"
        >画像を選択
        <input type="file" @change="onFileChange" />
      </label>
      <div class="preview-item">
        <img
          v-show="uploadedImage"
          class="preview-item-file"
          :src="uploadedImage"
          alt=""
        />
        <div v-show="uploadedImage" class="preview-item-btn" @click="remove">
          <p class="preview-item-name">{{ img_name }}</p>
          <e-icon class="preview-item-icon">close</e-icon>
        </div>
      </div>
    </div> -->
    <div id="app">
        <input @change="selectedFile" type="file" name="file">
        <button @click="upload" type="submit">アップロード</button>
    </div>

    </v-layout>
  </v-container>
</template>

<script>
import axios from 'axios';
export default {
  data() {
    uploadFile: null
  },
  methods: {
            selectedFile: function(e) {
                // 選択された File の情報を保存しておく
                e.preventDefault();
                let files = e.target.files;
                this.uploadFile = files[0];
            },
            upload: function() {
                // FormData を利用して File を POST する
                let formData = new FormData();
                formData.append('yourFileKey', this.uploadFile);
                let config = {
                    headers: {
                        'content-type': 'multipart/form-data'
                    }
                };
                axios
                    .post('yourUploadUrl', formData, config)
                    .then(function(response) {
                        // response 処理
                    })
                    .catch(function(error) {
                        // error 処理
                    })
            }
        }
  // uploadFile(e) {
  //     this.setState({
  //       file: e.target.files[0],
  //     }, () => {
  //       this.sendFile();
  //     });
  //   },
  //   sendFile() {
  //     // show loading modal
  //     this.setState({isLoading: true});
  //     const params = new FormData();
  //     params.append('file', this.state.file);
  //     axios
  //       .post(
  //         'https://noben.herokuapp.com/notes',
  //         params,
  //         {
  //           headers: {
  //             'content-type': 'multipart/form-data',
  //           },
  //         }
  //       )
  //       .then((result) => {
  //         this.setState({
  //           isLoading: false
  //         });
  //       },
  //       )
  //       .catch(() => {
  //         console.log('upload failed...');
  //         this.setState({
  //           isLoading: false
  //         });
  //       })
  //     }
  // async created() {
  //   try {
  //       await axios.get("https://noben.herokuapp.com/notes")
  //       .then((res) => {
  //         this.postImage = res.data[0].image_url;
  //       })
  //       .catch( (e) =>{
  //           console.log(e);
  //       });
  //   } catch (e) {
  //       console.log(e);
  //   }
  // }
};
</script>
