<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Login</router-link> |
      <router-link to="/about">About</router-link> | 
      <router-link to="/customers">Customer</router-link> |
      <router-link to="/projects">Projects</router-link>
    </div>
    <router-view />
  </div>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
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

.shadow {
    box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
}

.section {
    height: 100%;
    width: 75%;
    border: 1px solid #7593FF;
    background-color: #7593FF;
    border-radius: 15px;
    padding: 2%;
    margin: auto;
    margin-bottom: 25px;
}

.secondary-button {
    width: 137px;
    height: 36px;
    background: white;
    border-radius: 31px;
    border-color: white;
    box-shadow: none;
    color: #7593FF;
    font-weight: 600;
    margin-right: 15px;
}

.table {
  width: 80%;
  margin: auto;
}

.margin-right-md {
  margin-right: 15px;
}

</style>
