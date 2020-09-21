<template>
  <div id="app">
    <div id="nav">
      <Header v-bind:URL="URL" v-bind:loggedIn="loggedIn" @logout="logout"/>
      <router-view @loggedIn="login($event)"/>
    </div>
  </div>
</template>

<script>
import Header from './components/Header'

export default {
  name: 'App',
  components: {
    Header
  },
  data: function(){
    return {
      loggedIn: false,
      token: '',
      URL: 'https://p4backendjw.herokuapp.com/',
    }
  },
  methods: {
    login: function(event) {
      this.loggedIn = true
      this.token = event
      this.$router.push({
        path: "Main",
        query: { token: this.token.token, URL: this.URL },
      })
      window.localStorage.setItem('token', this.token.token)
    },
    logout: function(){
      this.loggedIn = false
      this.tokens = ''
      this.$router.push('/')
      window.localStorage.removeItem('token')
    }
  },
  beforeMount: function() {
    if (window.localStorage.getItem('token')) {
      this.token = window.localStorage.getItem('token')
      this.loggedIn = true
    } else {
      this.$router.push('/')
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  min-height: 100vh;
  color: #2c3e50;
}

#nav {
  padding: 10px 0px 100px 0px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}


</style>
