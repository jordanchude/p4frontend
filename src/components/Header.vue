<template>
  <div class="header">
    <b-navbar>
      <template slot="brand">
        <b-navbar-item tag="router-link" :to="{ path: '/' }">
          <span class="logo">CINEFAVORITES</span>
        </b-navbar-item>
      </template>
      

      <template slot="end">
        <b-navbar-item tag="div">
          <div class="buttons">
            <router-link :to="{ name: 'Login', query: { URL: this.URL }}" v-bind:URL="URL" v-if="!loggedIn"><button class="button is-link">Log in</button></router-link>
            <button  class="button is-link" v-if="loggedIn" @click="logout">Logout</button>
          </div>
        </b-navbar-item>
      </template>
    </b-navbar>
  </div>
</template>

<script>
export default {
  name: "Header",
  props: ['URL', 'loggedIn'],
  methods: {
    logout: function() {
      this.$emit('logout')
    }
  }
};
</script>

<style>
.header {
    width: 90%;
    margin: 10px auto
}

</style>
