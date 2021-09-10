<template>
  <router-view/>
</template>

<style>
@font-face {
  font-family: "warcraft";
  src: local("warcraft"),
   url(../public/warcraft.woff2);
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #eeeeee;
  font-family: 'warcraft', 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
  font-weight: 100;
  font-style: normal;
}

body {
  background-color: #111111;
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
</style>
