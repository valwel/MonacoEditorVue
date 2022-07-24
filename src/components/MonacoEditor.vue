<template>

  <div class="container">

    <div class="link"><a class="link__href" href="">Назад</a></div>

    <div class="upload">
      <input class="form-control" type="file" id="formFile" @change="($event) => onChange($event, 'original')">
      <input class="form-control" type="file" id="formFile" @change="($event) => onChange($event, 'modified')">
    </div>

    <MonacoEditor 
    :diffEditor="true" 
    :original="original" 
    :value="modified" 
    height="624" 
    width="1188" 
    id="editor" />
    <button class="button" @click="onClick">Проанализировать</button>
    <img class="loader" src="@/assets/loading.svg" alt="" v-if="isLoading">
  </div>

</template>
<script>
import MonacoEditor from "monaco-editor-vue";
export default {
  data() {
    return {
      original: ``,
      modified: ``,
      isLoading: false
    };
  },
  components: { MonacoEditor },
  methods: {

    onChange(e, updatedFileType) {
      const file = e.target.files[0];
      if (file) {
        const reader = new FileReader();
        reader.readAsText(file, "UTF-8");
        reader.onload = (evt) => {
          if (updatedFileType === 'original') {
            this.original = evt.target.result
          } else if (updatedFileType === 'modified') {
            this.modified = evt.target.result
          }
        }
      }
    },

    onClick() {
      this.isLoading = true;
      fetch('https://d5d8ctsk5ep2cvlh7bka.apigw.yandexcloud.net/api/code-comparator/compare', {
        method: 'POST',
        cors: true,
        body: JSON.stringify({
          "origin": this.original,
          "reviewer": this.modified
        })
      }).then((data) => {
        console.log(data);
      });
      setTimeout(() => this.isLoading = false, 1000)
    }
  }
};
</script>

<style lang="scss">
body {
  margin: 0;
  padding: 0;
  background-color: #F5F5F5;
}

.container {
  width: 100%;
  margin-left: 90px;
  font-size: 20px;
}

.link {
  margin-top: 25px;

  &__href {
    color: #666666;
    text-decoration: none;
  }
}

#editor {
  border: 1px solid #DEDEDE;
  margin-top: 20px;
}

.button {
  width: 213px;
  height: 51px;
  background: #424BB2;
  border-radius: 2px;
  color: #fff;
  font-size: 18px;
  margin: 20px 0;
  cursor: pointer;
}

.diffOverview {
  display: none;
}

.editor.modified {
  margin-left: 60px;
  height: 624px;
  width: 594px;
}

.upload {
  display: flex;
  margin-top: 50px;
}

.upload :nth-child(n+2) {
  margin-left: 60px;
}

#formFile {
  width: 578px;
}

.loader {
  margin-left: 30px;
  animation: loading .5s infinite linear;
}

@keyframes loading {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}
</style>