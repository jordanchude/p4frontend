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
    
    <!-- SIGN UP -->
    <b-field label="Email"
        type="is-success"
        message="This username is available">
        <b-input type="email" maxlength="30" v-model="newEmail"></b-input>
    </b-field>

    <b-field label="Username"
        type="is-warning"
        :message="['Password is too short', 'Password must have at least 8 characters']">
        <b-input type="text" maxlength="30" v-model="newUsername"></b-input>
    </b-field>

    <b-field label="Password"
        type="is-warning"
        :message="['Password is too short', 'Password must have at least 8 characters']">
        <b-input type="password" maxlength="30" v-model="newPassword"></b-input>
    </b-field>

    <b-field label="First Name"
        type="is-warning"
        :message="['Password is too short', 'Password must have at least 8 characters']">
        <b-input maxlength="30" v-model="newFirstName"></b-input>
    </b-field>

    <b-field label="Last Name"
        type="is-warning"
        :message="['Password is too short', 'Password must have at least 8 characters']">
        <b-input type="text" maxlength="30" v-model="newLastName"></b-input>
    </b-field>
    <button class="button is-danger" @click="handleSignup">Sign Up</button>
  </div>
</template>

<script>
export default {
  name: 'Login',
  data: function() {
    return {
       username: '',
       password: '',
       newEmail: '',
       newUsername: '',
       newPassword: '',
       newFirstName: '',
       newLastName: '',
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
        if (data) {
          this.$emit('loggedIn', data)
        } else {
          alert('Incorrect Login')
        }
      })
    },
    handleSignup: function() {
        const URL = this.$route.query.URL;
        const user = { 
          email: this.newEmail,
          username: this.newUsername,
          password: this.newPassword,
          first_name: this.newFirstName,
          last_name: this.newLastName
        };
        fetch(`${URL}auth/users/register/`,{
        method: "POST",
        headers: {
            "Content-Type": "application/json"
        },
        body: JSON.stringify(user)
        })
        .then (response => {
          if (response.status !== 200) {
            response.json()
          } else {
            return response.json()
          }
        })
        .then((data) => {
        if (data){
            alert("sign up successful");
            this.newEmail = ''
            this.newUsername = ''
            this.newPassword = ''
            this.newFirstName = ''
            this.newLastName = ''
        } else {
            alert("sign up unsuccessful");
        }
      });
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
