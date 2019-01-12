<template>
  <div id="app">
    <div id="nav">
      <router-link style="text-decoration: none;" class="rout" to="/"><h4>Ambucs</h4></router-link> 
      <router-link style="text-decoration: none;" class="rout" to="/">Home</router-link> 
      <router-link style="text-decoration: none;" class="rout" to="/users">Users</router-link>
    </div>
    <router-view/>
  </div>
</template>

<style>
#nav {
  list-style-type: none;
  margin: 0;
  padding: 0;
  overflow: hidden;
  background-color: #333;
}

.rout{
  float: left;
  display: block;
  color: white;
  text-align: center;
  padding: 14px 16px;
  text-decoration: none;
}

.rout:hover:not(.active) {
  background-color: #111;
}

.active {
  background-color: #4CAF50;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

</style>
