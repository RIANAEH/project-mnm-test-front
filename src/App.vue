<template>
  <v-app>
    <v-app-bar
      app
      color="teal lighten-3"
      dark
    >
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="./assets/logo_mnm.png"
          transition="scale-transition"
          width="150"
        />
      </div>

      <v-spacer></v-spacer>

      <v-btn
        href="https://github.com/RIANAEH/project-mnm-test-front"
        target="_blank"
        text
      >
        <span class="mr-2">GitHub</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn>
    </v-app-bar>

    <v-content>
      <v-alert
        dense
        type="info"
        color="teal"
      >
        Spring 서버와 Django 서버에서 제공하는 REST API를 테스트 할 수 있습니다.
      </v-alert>
      <v-expansion-panels>
        <AuthView/>
        <ImageView/>
        <MatchingView/>
        <MatchingResultView/>
      </v-expansion-panels>
    </v-content>
  </v-app>
</template>

<script>
import ImageView from './components/ImageView.vue';
import AuthView from './components/AuthView.vue';
import MatchingView from './components/MatchingView.vue'
import MatchingResultView from './components/MatchingResultView.vue'

export default {
  name: 'App',

  components: {
    ImageView,
    AuthView,
    MatchingView,
    MatchingResultView
  },

  data: () => ({
    //
  }),
};
</script>
