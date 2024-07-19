<template>
  <v-app style="background:url('public/gradient-music-notes-background_23-2151270122.avif')">
>
<v-app-bar app color="black">
<v-btn to="/">Home</v-btn>
      <v-toolbar-title>My Music App</v-toolbar-title>
    </v-app-bar>
  <v-container>
    <div id="app">
    <MusicPlayer />
  </div>
  </v-container>
 </v-app>
</template>

<script>
import MusicPlayer from '@/components/MusicPlayer.vue';

export default {
  name: 'App',
  components: {
    MusicPlayer
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>