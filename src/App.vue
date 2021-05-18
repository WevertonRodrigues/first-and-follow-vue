<template>
  <div class="d-flex flex-col flex-center">
    <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </div>
    <div id="main-app">
      <router-view />
    </div>
  </div>
</template>

<style>
#main-app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  display: flex;
  flex-direction: column;
  align-items: center !important;
  justify-content: center !important;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}

.d-flex {
  display: flex;
}

.flex-col {
  flex-direction: column !important;
}

.flex-center {
  align-items: center !important;
  justify-content: center !important;
}

.margin-right {
  margin-right: 2px;
}
</style>
