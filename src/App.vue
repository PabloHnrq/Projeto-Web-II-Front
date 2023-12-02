<template>
  <div id="app">
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: 'App',
};
</script>

<style>

html, body {
  height: 100vh;
  margin: 0;
  padding: 0;
  }

#app {
  background-image: url(https://www.10wallpaper.com/wallpaper/1366x768/1605/Purple_calm_ocean_dusk-Nature_Scenery_HD_Wallpaper_1366x768.jpg);
  background-position: center center;
  background-repeat: no-repeat;
  background-size: cover;
  height: 100vh;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;

}


</style>
