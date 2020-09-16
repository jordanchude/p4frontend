<template>
  <div class="login">
    <b-field label="Username"
        type="is-success"
        message="This username is available">
        <b-input maxlength="30" v-model="username"></b-input>
    </b-field>

    <b-field label="Password"
        type="is-warning"
        :message="['Password is too short', 'Password must have at least 8 characters']">
        <b-input type="password" maxlength="30" v-model="password"></b-input>
    </b-field>
    <button class="button is-danger" @click="handleLogin">Login</button>
  </div>
</template>

<script>
export default {
  name: 'Login',
  data: function() {
    return {
       username: '',
       password: ''
    };
  },
  methods: {
    handleLogin: function(){
      fetch(`${this.$route.query.URL}auth/users/login/`, {
        method: 'post',
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify({
          username: this.username,
          password: this.password
        })
      })
      .then (response => {
        if (response.status !== 200) {
          response.json()
        } else {
          return response.json()
        }
      })
      .then(data => {
        console.log('data', data)
        if (data) {
          this.$emit('loggedIn', data)
        } else {
          alert('Incorrect Login')
        }
      })
    }
  }
}
</script>

<style>
.login {
  width: 70%;
  margin: 10px auto;
}
</style>
