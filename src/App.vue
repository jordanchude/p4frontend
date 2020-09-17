<template>
  <div id="app">
    <div id="nav">
      <Header v-bind:URL="URL" v-bind:loggedIn="loggedIn" @logout="logout"/>
      <router-view @loggedIn="login($event)"/>
      <Footer/>
    </div>
  </div>
</template>

<script>
import Header from './components/Header'
import Footer from './components/Footer'

export default {
  name: 'App',
  components: {
    Header,
    Footer
  },
  data: function(){
    return {
      loggedIn: false,
      token: '',
      URL: 'http://127.0.0.1:8000/',
    }
  },
  methods: {
    login: function(event) {
      this.loggedIn = true
      this.token = event
      this.$router.push({
        path: "Main",
        query: { token: this.token.token, URL: this.URL },
      });
    },
    logout: function(){
      this.loggedIn = false
      this.tokens = ''
      this.$router.push('/')
    },
    showMovie: function(){
      this.$router.push('/movie')
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
</style>
