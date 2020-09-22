<template>
  <div class="login">
    <!-- LOGIN -->
    <b-collapse class="card" animation="slide" aria-id="contentIdForA11y3">
            <div
                slot="trigger" 
                class="card-header"
                role="button"
                aria-controls="contentIdForA11y3">
                <p class="card-header-title">
                    LOGIN
                </p>
            </div>
            <div class="card-content">
                <div class="content">
                  <b-field label="Username">
                      <b-input maxlength="30" v-model="username"></b-input>
                  </b-field>

                  <b-field label="Password">
                      <b-input type="password" maxlength="30" v-model="password"></b-input>
                  </b-field>
                  <button class="button is-danger" @click="handleLogin">Login</button>
                </div>
            </div>
        </b-collapse>

    <!-- SIGN UP -->
    <b-collapse class="card" animation="slide" aria-id="contentIdForA11y3">
            <div
                slot="trigger" 
                class="card-header"
                role="button"
                aria-controls="contentIdForA11y3">
                <p class="card-header-title">
                    SIGN UP
                </p>
            </div>
            <div class="card-content">
                <div class="content">
                  <b-field label="Email">
                      <b-input type="email" maxlength="30" v-model="newEmail"></b-input>
                  </b-field>

                  <b-field label="Username">
                      <b-input type="text" maxlength="30" v-model="newUsername"></b-input>
                  </b-field>

                  <b-field label="Password">
                      <b-input type="password" minlength="8" maxlength="30" v-model="newPassword"></b-input>
                  </b-field>

                  <b-field label="First Name">
                      <b-input maxlength="30" v-model="newFirstName"></b-input>
                  </b-field>

                  <b-field label="Last Name">
                      <b-input type="text" maxlength="30" v-model="newLastName"></b-input>
                  </b-field>
                  <button class="button is-danger" @click="handleSignup">Sign Up</button>
                              </div>
                          </div>
                      </b-collapse>   
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
          if (response.status == 400) {
            alert('sign up unsuccessful')
            response.json()
          } else {
            alert('sign up successful')
            this.newEmail = ''
            this.newUsername = ''
            this.newPassword = ''
            this.newFirstName = ''
            this.newLastName = ''
            return response.json()
          }
        })
    }
  }
}
</script>

<style>
#login-text {
  font-size: 30px;
  margin-bottom: 30px;
}

.card-header-title {
  font-size: 30px;
  text-align: center;
  display: flex;
  align-items: center;
  justify-content: center;
}

.card {
  margin: 30px auto auto auto;
}
</style>
