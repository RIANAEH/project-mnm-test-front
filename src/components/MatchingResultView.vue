<template>
    <v-container>
        <v-expansion-panel>
        <v-expansion-panel-header><h1>메이트 매칭 결과</h1></v-expansion-panel-header>
        <v-expansion-panel-content>
        <v-card class="mt-2">
            <v-card-title><h2>메이트 매칭 결과 요청</h2></v-card-title>
            <v-card-text>
                  <v-form>
                    <v-container>
                        <v-row>
                            <v-col
                                cols="12"
                                md="4"
                            >
                                <v-text-field
                                    v-model="post.uid"
                                    label="User ID"
                                    required
                                ></v-text-field>
                            </v-col>
                        </v-row>
                        <v-btn dark color="teal lighten-3" @click="postMatchingResult">POST matching result</v-btn>
                    </v-container>
                </v-form>
                <v-container>
                    <div v-if='post.view === true'>{{ post.status }}</div>
                    <div v-if='post.view === true'>{{ post.data }}</div>
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
                post : {
                    uid: '',
                    status: '', 
                    data: '', 
                    view: false
                }
            }
        },
        methods: {
            postMatchingResult() {
                axios.post('http://localhost:5000/results/' + this.post.uid)
                .then((res) => {
                    console.log(`status code: ${res.status}`);
                    console.log(`data: ${res.data}`)

                    this.post.status = res.status;
                    this.post.data = res.data;
                    this.post.view = true;
                });
            }
        }
    }
</script>