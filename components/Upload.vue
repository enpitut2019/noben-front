<template>
<v-container>
  <v-layout text-center wrap>

    <div id="app">
      <input type="file" @change="onFileChanged">
      <button @click="onUpload">Upload</button>
      <div v-for="(image,index) in images">
      <h2>{{image.name}}</h2>
      <img v-show="image" :src="image.thumbnail" />
      <!-- <div v-show="image" class="preview-item-btn" @click="remove(index)">
        <button class="preview-item-icon">close</button>
      </div> -->

      </form> -->
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
        obj.file = file;
        obj.name = file.name;
        this.images.push(obj);
      };
      reader.readAsDataURL(file);
    },
    // remove(index) {
    //   this.images[index] = null;
    // },
  }

};
</script>

<style>
  button {
        color: black;
        background-color: white;
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

    #app {
      width: 100%;
    }

    #app img {
      width: 80%;
    }
</style>
