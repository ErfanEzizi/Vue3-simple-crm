<template>
  <nav class="nav_bar">
    <div class="left">
      <div class="app-logo-title">
        <img
          src="https://avatars3.githubusercontent.com/u/37439169?s=200&v=4"
          alt="app_logo"
        />
        <span>DEV APP</span>
      </div>
      <div>
        <router-link
          class="nav_link"
          :class="{ active: $route.path === '/' }"
          to="/"
          >Home</router-link
        >
        <router-link
          class="nav_link"
          :class="{ active: $route.path === '/customers' }"
          to="/customers"
          >Customers</router-link
        >
      </div>
    </div>
    <div class="right">
      <img src="../assets/avatar.png" alt="avatar" />
      <div>
        <h4>Erfan Ezizi</h4>
        <p>something funny?</p>
      </div>
    </div>
  </nav>
</template>

<script>
export default {
  name: "NavigationBar",
};
</script>

<style scoped>
h1,
h4 {
  margin: 0;
}

p {
  margin: 0;
  opacity: 0.5;
}
.nav_bar {
  display: flex;
  justify-content: space-between;
  border-bottom: 2px solid #a4aabd;
  padding-bottom: 5px;
  margin-bottom: 5px;
}

.left {
  display: flex;
  align-items: center;
  gap: 15px;
}

.app-logo-title {
  display: flex;
  align-items: center;
  font-family: "Lato", sans-serif;
  font-size: xx-large;
  color: #1c2c54;
}
.app-logo-title > img {
  width: 60px;
  height: 60px;
}

.right {
  display: flex;
  gap: 15px;
  align-items: center;
}

.right > img {
  width: 30px;
  height: 30px;
  border-radius: 100%;
}

.nav_link {
  padding: 5px;
  text-decoration: none;
  color: #1c2c54;
}
.active {
  color: #4451FE;
  border-bottom: 2px solid #2050fe;
  font-weight: bolder;
}
</style>

