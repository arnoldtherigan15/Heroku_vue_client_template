<template>
    <div class="containerMain">
        <div>
          <div class="titleText">
            <h1>Image Link Generator</h1>
          </div>
          <b-field class="cardUpload">
              <b-upload v-model="file" style="width:40vw;">
                  <a class="button is-primary">
                      <b-icon icon="upload"></b-icon>
                      <span>Choose your image</span>
                  </a>
              </b-upload>
              <span class="file-name" v-if="file" style="margin-top:1rem;">
                  {{ file.name }}
              </span>
          </b-field>
          <b-button @click.prevent="upload" type="is-primary" style="float:right; width: 200px;">Upload</b-button>
        </div>
        <div v-if="result" style="display:flex;justify-content:center; background-color:#b7bebf; padding:1rem;">
            <a :href="result" target="_blank" style="text-align:center; color:white; text-shadow: 1px 1px black;">{{result}}</a>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import Swal from 'sweetalert2'
    export default {
        name: 'App',
        data() {
            return {
                isFullPage: true,
                file: null,
                result: null
            }
        },
        methods: {
            upload() {
              let bodyFormData = new FormData()
              bodyFormData.append('url', this.file)
              const loadingComponent = this.$buefy.loading.open({
                    container: this.isFullPage ? null : this.$refs.element.$el
              })
              axios({
                method: 'post',
                url: '/api/upload',
                data: bodyFormData
              })
                .then(({ data }) => {
                  this.file = ''
                  this.result = data.url
                  loadingComponent.close()
                  Swal.fire('success', 'image uploaded', 'success')

                })
                .catch(err => {
                  loadingComponent.close()
                  Swal.fire('error', 'Make sure you upload image file', 'error')
                })
            }
        }
    }
</script>

<style>
.containerMain {
  width: 100vw;
  height: 100vh;
  overflow: hidden;
  background-image: url('./assets/bg.jpg');
  background-position: center;
  background-size: cover;
  background-repeat: no-repeat;
  display:flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

.tags {
  margin-top: 1rem; 
}

.cardUpload {
  background-color:rgba(223, 230, 233,.8);
  border:1px dashed black;
}

.titleText {
  background-color:rgba(223, 230, 233,.8);
  padding: 1rem;
  font-size:3rem;
  text-align: center;
  color:white;
  text-shadow: 2px 2px black;
}
</style>