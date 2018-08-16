<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Home</router-link> | 
      <router-link to="/about">About</router-link>
      <span v-if="isLoggedIn"> | <a @click="logout">Logout</a></span>
      <span v-if="!isLoggedIn"> | <router-link to="/login">Login</router-link></span>
    </div>
    <h2 v-if="isLoggedIn">Welcome, {{user.name}}</h2>
    <router-view/>
  </div>
</template>

<script>
export default {
  computed: {
    isLoggedIn : function() {
      return this.$store.getters.isLoggedIn;
    },
    user : function() {
      return this.$store.state.user;
    }
  },
  methods: {
    logout: function () {
      this.$store.dispatch('logout')
        .then(() => this.$router.push('/login'))
    }
  },
  created: function () {
    this.$http.interceptors.response.use(undefined, function(err) {
      return new Promise(function(resolve, reject) {
        if (err.status === 401 && err.config && !err.config.__isRetryRequest) {
          this.$store.dispatch('logout')
        } 
        throw err;
      })
    })
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
#nav {
  padding: 30px;
}
#nav a {
  font-weight: bold;
  color: #2c3e50;
  cursor: pointer;
}
#nav a:hover {
  text-decoration: underline;
}
#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
