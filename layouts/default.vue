<template>
  <v-app dark>
    <v-app-bar
      
      app
      color="indigo lighten-5"
    >
      <v-toolbar-title><b>{{ title }}</b> v1.0</v-toolbar-title>
      <v-spacer />
    </v-app-bar>
    <v-main>
      <v-container>
        <nuxt />
      </v-container>
    </v-main>
    <v-footer
      
      color="blue lighten-5"
    >
      <span><i>for</i> Tugas Besar <b>II4042 - Kecerdasan Buatan untuk Bisnis </b></span>
      <span>18217005 - 18217011 - 18217042 &copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  data () {
    return {
      title: 'House Price Predictor System'
    }
  }
}
</script>
