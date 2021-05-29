<template>
  <div class="d-flex flex-col flex-center">
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

.margin-x {
  margin-right: 2px;
  margin-left: 2px;
}
</style>
