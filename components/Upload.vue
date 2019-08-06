<template>
<v-container>
  <v-layout text-center wrap>

    <div id="app">
    <div class="fm">
      <input type="file" @change="onFileChanged" multiple>
      <button @click="onUpload">アップロード</button>
      <div v-for="image in images">
        <h2>{{image.name}}</h2>
        <img v-show="image" :src="image.thumbnail" />
      </div>
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
      uploadedImages: [],
      images: []
    }
  },
  methods: {
    onFileChanged(event) {
      this.uploadedImages = event.target.files
      for(var i = 0;i < this.uploadedImages.length;i++) {
        this.createImage(this.uploadedImages[i])
      }
    },
    onUpload() {
      const formData = new FormData()
      formData.append('note[subject_name]', 'hoge')
      for (  var i = 0;  i < this.images.length;  i++  ) {
        formData.append(`note[pages_attributes][${i}][image]`, this.images[i].image, this.images[i].name)
        formData.append(`note[pages_attributes][${i}][order]`, 0)
      }
      formData.append('tags', 'aaa, bbb, ccc')
      axios.post('https://noben.herokuapp.com/notes', formData)
    },
    createImage(image) {
      const reader = new FileReader();
      let obj = {};
      reader.onload = e => {
        obj.thumbnail = e.target.result;
        obj.image = image
        obj.name = image.name;
        this.images.push(obj);
      };
      reader.readAsDataURL(image);
    },
    remove() {
      this.sFile = false;
    },
  }

};
</script>

<style>
  .fm button {
        color: black;
        background-color: white;
        /*margin: auto;
        position: absolute;*/
        border:none;
        border-radius:10px;
        box-shadow: none;
        padding: 2px 8px;
    }

    .fm input, button:focus {
        outline: none;
    }

    #app {
      width: 100%;
    }

    #app img {
      width: 80%;
    }
</style>
