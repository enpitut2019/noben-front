<template>
<v-container>
  <v-layout text-center wrap>

    <div id="app">
			<v-form ref = "form">
				<v-text-field
					v-model = "subjectName"
					label = "教科名"
					placeholder = "線形代数"
					required
					></v-text-field>
        <v-icon>mdi-anchor</v-icon>
				<v-text-field
					v-model = "tags"
					prepend-icon="close"
					label = "タグ"
					placeholder = "3回目, 課題"
					required
					></v-text-field>
				<v-file-input
				  label="ノート画像"
			    @change = "onFileChanged"
          multiple
					></v-file-input>
			</v-form>
      <div>
        <input type="file" @change="onFileChanged" multiple>
        <button @click="onUpload">Upload</button>
      </div>
      <div v-for="(image,index) in images">
        <h2>{{image.name}}</h2>
        <img v-show="image" :src="image.thumbnail" />
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
      subjectName: '',
      tags: '',
      images: []
    }
  },
  methods: {
    onFileChanged(event) {
      let files = event.target.files;
      for(var i = 0;i < files.length;i++) {
        this.createImage(files[i])
      }
    },
    onUpload() {
      const formData = new FormData()
      formData.append('note[subject_name]', this.subjectName)
      for (  var i = 0;  i < this.images.length;  i++  ) {
        formData.append(`note[pages_attributes][${i}][image]`, this.images[i].file, this.images[i].name)
        formData.append(`note[pages_attributes][${i}][order]`, 0)
      }
      formData.append('tags', this.tags)
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
  .string-input {
    color: black;
    background-color: white;
  }

  .string-input::placeholder {
    color: gray;
  }

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
