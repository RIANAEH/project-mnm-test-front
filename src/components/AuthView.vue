<template>
    <v-container>
        <v-expansion-panel>
        <v-expansion-panel-header>
            <span>
            <v-badge
                :value="loginCheck"
                bordered
                color="success"
                icon="mdi-checkbox-marked-circle"
                overlap
            >
                <h1>회원 관리</h1>
            </v-badge>
            </span>
        </v-expansion-panel-header>
        <v-expansion-panel-content>
        <v-card class="mt-2">
            <v-card-title><h2>회원가입</h2></v-card-title>
            <v-card-text>
                  <v-form>
                    <v-container>
                        <v-row>
                            <v-col
                                cols="12"
                                md="4"
                            >
                                <v-text-field
                                    v-model="join.email"
                                    label="Email"
                                    required
                                ></v-text-field>
                            </v-col>
                            <v-col
                                cols="12"
                                md="4"
                            >
                                <v-text-field
                                    v-model="join.password"
                                    label="Password"
                                    required
                                ></v-text-field>
                            </v-col>
                        </v-row>
                        <v-btn @click="postJoin">POST join</v-btn>
                    </v-container>
                </v-form>
                <v-container>
                    <div v-if='join.view === true'>{{ join.response }}</div>
                    <div v-if='join.view === true'>{{ join.message }}</div>
                    <div v-if='join.view === true'>{{ join.data }}</div>
                </v-container>
            </v-card-text>
        </v-card>
        <v-card class="mt-2">
            <v-card-title><h2>로그인</h2></v-card-title>
                <v-card-text>
                  <v-form>
                    <v-container>
                        <v-row>
                            <v-col
                                cols="12"
                                md="4"
                            >
                                <v-text-field
                                    v-model="login.email"
                                    label="Email"
                                    required
                                ></v-text-field>
                            </v-col>
                            <v-col
                                cols="12"
                                md="4"
                            >
                                <v-text-field
                                    v-model="login.password"
                                    label="Password"
                                    required
                                ></v-text-field>
                            </v-col>
                        </v-row>
                        <v-btn @click="postLogin">POST login</v-btn>
                    </v-container>
                </v-form>
                <v-container>
                    <div v-if='login.view === true'>{{ login.response }}</div>
                    <div v-if='login.view === true'>{{ login.message }}</div>
                    <div v-if='login.view === true'>{{ login.data }}</div>
                    <div v-if='login.view === true'>uid: {{ login.data.uid }}</div>
                    <div v-if='login.view === true'>JWT token: {{ login.data.token }}</div>
                </v-container>
            </v-card-text>
        </v-card>
        <v-card class="mt-2">
            <v-card-title><h2>로그아웃</h2></v-card-title>
                <v-card-text>
                    <v-container>
                        <v-btn @click="deleteToken">DELETE token</v-btn>
                    </v-container>
                <v-container>
                    <div v-if='logout.view === true'>token: {{ logout.response }}</div>
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
                loginCheck: 0,
                join : {
                    email: '',
                    password: '', 
                    response: '', 
                    message: '',
                    data: '', 
                    view: false
                },
                login: {
                    email: '', 
                    password: '',
                    response: '', 
                    message: '',
                    data: '', 
                    view: false                    
                },
                logout: {
                    response: '',
                    view: false
                }
            }
        },
        methods: {
            postJoin() {
                axios.post('http://localhost:5050/auth/join', {
                    email: this.join.email,
                    password: this.join.password
                })
                .then((res) => {
                    console.log(`status code: ${res.status}`);
                    console.log(`response: ${res.data.response}`);
                    console.log(`message: ${res.data.message}`);
                    console.log(`data: ${res.data.data}`)

                    this.join.response = res.data.response;
                    this.join.message = res.data.message;
                    this.join.data = res.data.data;
                    this.join.view = true;
                });
            },
            postLogin() {
                axios.post('http://localhost:5050/auth/login', {
                    email: this.login.email,
                    password: this.login.password,
                })
                .then((res) => {
                    console.log(`status code: ${res.status}`);
                    console.log(`response: ${res.data.response}`);
                    console.log(`message: ${res.data.message}`);
                    console.log(`data: ${res.data.data}`)

                    this.login.response = res.data.response;
                    this.login.message = res.data.message;
                    this.login.data = JSON.parse(res.data.data);
                    this.login.view = true;

                    // 로컬스토리지에 토큰 저장
                    localStorage.setItem('token', this.login.data.token);
                    console.log(localStorage.getItem('token'));

                    this.loginCheck = 1;
                })
                .catch(err => {
                    console.log(err);
                });
            },
            deleteToken() {
                localStorage.removeItem('token');
                if (localStorage.getItem('token') === null) this.logout.response = "success";
                else this.logout.response = "failed";
                this.logout.view = true;

                this.loginCheck = 0;
            }
        }
    }
</script>