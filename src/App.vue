<template>
  <div>
    <NavBar />
    <router-view></router-view>
  </div>
</template>

<script>
import NavBar from "./components/NavBar";
export default {
  name: "App",
  components: {
    NavBar: NavBar,
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=IBM+Plex+Sans:wght@300&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Lato&display=swap");

body {
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: #fafafa;
}
#app {
  font-family: "IBM plex sans";
  width: 70vw;
  padding: 20px;
  border-bottom: 2px solid #a4aabd;
}
</style>
