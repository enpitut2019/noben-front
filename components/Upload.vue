<template>
<v-container>
  <v-layout text-center wrap>

    <div id="app">
			<v-form ref = "form">
				<v-text-field
					v-model = "subjectName"
					label = "教科名"
					placeholder = "教科名を入力（例：線形代数）"
					required
					></v-text-field>
				<v-text-field
					v-model = "tags"
					label = "タグ"
					placeholder = "半角カンマ区切りでタグを入力（例：3回目, 課題, 解説求む）"
					required
					></v-text-field>
				<!-- <v-file-input
				  label="ノート画像"
			    @change = "onFileChanged"
          multiple
					></v-file-input> -->
			</v-form>
      <div>
        <input type="file" @change="onFileChanged" multiple>
        <!-- <div v-if="images!==null"> -->
        <button @click="onUpload" v-if="images.length!=0">Upload</button>
      <!-- </div> -->
      </div>
      <div v-for="(image,index) in images">
        <h2>{{image.name}}</h2>
        <img v-show="image" :src="image.thumbnail" />
        <!-- <h2>{{index}}</h2>
        <h2>{{images[index].name}}</h2> -->
      <div v-show="images" class="preview-item-btn" @click="remove(`${index}`)">
          <button class="preview-item-icon">delete</button>
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
      console.log(this.images)
      const formData = new FormData()
      formData.append('note[subject_name]', this.subjectName)
      for (  var i = 0;  i < this.images.length;  i++  ) {
        formData.append(`note[pages_attributes][${i}][image]`, this.images[i].file, this.images[i].name)
        formData.append(`note[pages_attributes][${i}][order]`, 0)
      }
      formData.append('tags', this.tags)
      axios.post('https://noben.herokuapp.com/notes', formData)
           .then((res) => {
             this.$router.push({ name: 'DetailPage-id', params: { id: res.data.id }})
           })
      this.images = []
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
    remove(index) {
      this.images.splice(Number(index), 1);
      console.log(index);
    },
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
