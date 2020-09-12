<template>
  <div id="app">
    <appHeader />
    <photos />
  </div>
</template>

<script>
import appHeader from "./components/appHeader";
import photos from "./components/Photos";
export default {
  name: "App",
  components: {
    appHeader,
    photos,
  },
};
</script>

<style>
*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body,
html {
  height: 100%;
}
body {
  font-family: "Montserrat", sans-serif;
}
</style>
