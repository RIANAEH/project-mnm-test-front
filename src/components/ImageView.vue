<template>
    <v-container>
        <v-expansion-panel>
        <v-expansion-panel-header><h1>이미지 관리</h1></v-expansion-panel-header>
        <v-expansion-panel-content>
        <v-card class="mt-2">
          <v-card-title><h2>이미지 불러오기</h2></v-card-title>
          <v-card-text>
            <v-container> 
              <v-form>
                <v-container>
                    <v-row>
                        <v-col
                            cols="12"
                            md="4"
                        >
                            <v-text-field
                                v-model="get.imagePath"
                                label="Image Path"
                                required
                            ></v-text-field>
                        </v-col>
                    </v-row>
                    <v-btn dark color="teal lighten-3" @click="getImage">GET image</v-btn>
                  </v-container>
              </v-form> 
              <v-img 
                class="mt-2"
                v-if='get.view === true'
                max-height="200" 
                max-width="200"
                v-bind:src="get.imageSrc"
              ></v-img>
            </v-container>
          </v-card-text>
        </v-card>
    </v-expansion-panel-content>
    </v-expansion-panel>
    </v-container>
</template>

<script lang="js">
import axios from 'axios'

    export default {
        data() {
            return {
              get: {
                imagePath: '',
                imageSrc: '', 
                view: false
              }
            }
        },
        methods: {
          getImage() {
            axios.get('http://localhost:5050/user/image', {
              params: { imagePath: this.get.imagePath },
              headers: { 'X-AUTH-TOKEN': localStorage.getItem('token') },
              timeout: 1000 // 1초 이내에 응답이 없으면 에러 처리
            })
            .then((res) => {
              console.log(`status code: ${res.status}`);
              console.log(`response: ${res.data.response}`);
              console.log(`message: ${res.data.message}`);
              console.log(`data: ${res.data.data}`)

              this.get.imageSrc = "data:image/jpg;base64," + res.data.data;
              this.get.view = true;
            });
          }
        }
    }
</script>