<template>
    <v-container>
        <v-expansion-panel>
        <v-expansion-panel-header><h1>매칭 정보 관리</h1></v-expansion-panel-header>
        <v-expansion-panel-content>
        <v-card class="mt-2">
          <v-card-title><h2>매칭 정보 등록</h2></v-card-title>
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
                    <v-alert
                        border="left"
                        colored-border
                        icon="mdi-firework"
                        color="teal lighten-3 accent-5"
                        elevation="2"
                    >
                        당신의 성별은..?
                        <v-radio-group
                            v-model="post.sex"
                            row
                        >
                            <v-radio
                                label="여자"
                                color="teal lighten-3"
                                value="1"
                            ></v-radio>
                            <v-radio
                                label="남자"
                                color="teal lighten-3"
                                value="0"
                            ></v-radio>
                        </v-radio-group>
                    </v-alert>
                    <v-alert
                        border="left"
                        colored-border
                        icon="mdi-firework"
                        color="teal lighten-3 accent-5"
                        elevation="2"
                    >
                        당신의 MBTI는..?
                        <v-radio-group
                            v-model="post.mbti"
                            row
                        >
                            <v-radio
                                label="intp"
                                color="teal lighten-3"
                                value="intp"
                            ></v-radio>
                            <v-radio
                                label="enfp"
                                color="teal lighten-3"
                                value="enfp"
                            ></v-radio>
                        </v-radio-group>
                    </v-alert>
                    <v-alert
                        border="left"
                        colored-border
                        icon="mdi-firework"
                        color="teal lighten-3 accent-5"
                        elevation="2"
                    >
                        나의 하우스 메이트는..?
                        <v-radio-group
                            v-model="post.mateSmoking"
                            row
                        >
                            <v-radio
                                label="흡연자여야 한다."
                                color="teal lighten-3"
                                value="1"
                            ></v-radio>
                            <v-radio
                                label="비흡연자여야 한다."
                                color="teal lighten-3"
                                value="2"
                            ></v-radio>
                            <v-radio
                                label="흡연여부는 상관 없다."
                                color="teal lighten-3"
                                value="3"
                            ></v-radio>
                        </v-radio-group>
                    </v-alert>
                    <v-btn dark color="teal lighten-3" @click="postMatchingInfo">POST matchinginfo</v-btn>
                </v-container>
            </v-form>
            <v-container>
                <div v-if='post.view === true'>{{ post.response }}</div>
                <div v-if='post.view === true'>{{ post.message }}</div>
                <div v-if='post.view === true'>{{ post.data }}</div>
            </v-container>
          </v-card-text>
        </v-card>
        <v-card class="mt-2">
          <v-card-title><h2>매칭 정보 조회</h2></v-card-title>
          <v-card-text>
            <v-form>
                <v-container>
                    <v-row>
                        <v-col
                            cols="12"
                            md="4"
                        >
                            <v-text-field
                                v-model="get.uid"
                                label="User ID"
                                required
                            ></v-text-field>
                        </v-col>
                    </v-row>
                    <v-btn dark color="teal lighten-3" @click="getMatchingInfo">GET matchinginfo</v-btn>
                </v-container>
            </v-form>
            <v-container>
                <div v-if='get.view === true'>{{ get.response }}</div>
                <div v-if='get.view === true'>{{ get.message }}</div>
                <div v-if='get.view === true'>{{ get.data }}</div>
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
                    uid: '',
                    response: '', 
                    message: '',
                    data: '', 
                    view: false
                },
                post: {
                    uid: '',
                    mbti: '',
                    mateSmoking: '',
                    sex: '',
                    response: '', 
                    message: '',
                    data: '', 
                    view: false
                }
            }
        },
        methods: {
            postMatchingInfo() {
                axios.post('http://localhost:5050/user/matchinginfo/', {
                    user: {
                        id: this.post.uid
                    },
                    mbti: this.post.mbti,
                    mateSmoking: this.post.mateSmoking
                }, { 
                    headers: {
                        'X-AUTH-TOKEN': localStorage.getItem('token')
                    }
                    })
                .then((res) => {
                console.log(`status code: ${res.status}`);
                console.log(`response: ${res.data.response}`);
                console.log(`message: ${res.data.message}`);
                console.log(`data: ${res.data.data}`)

                this.post.response = res.data.response;
                this.post.message = res.data.message;
                this.post.data = res.data.data;
                this.post.view = true;
                });
            },
            getMatchingInfo() {
                axios.get('http://localhost:5050/user/matchinginfo/' + this.get.uid, {
                    headers: { 'X-AUTH-TOKEN': localStorage.getItem('token') }
                })
                .then((res) => {
                console.log(`status code: ${res.status}`);
                console.log(`response: ${res.data.response}`);
                console.log(`message: ${res.data.message}`);
                console.log(`data: ${res.data.data}`)

                this.get.response = res.data.response;
                this.get.message = res.data.message;
                this.get.data = res.data.data;
                this.get.view = true;

                console.log(res.data.data.mbti); // 세부 정보 이렇게 접근
                });
            }
        }
    }
</script>