<template>
  <div>
    <b-button v-b-toggle.sidebar-no-header>Login</b-button>
    <b-sidebar id="sidebar-no-header"
    aria-labelledby="sidebar-no-header-title"
    no-header shadow>
      <template #default="{ hide }">
        <div class="card">
            <div class="bg"></div>
            <img src="../assets/asetLogin.png" class="card-img-top img-thumbnail " alt="...">
            <p class="text-center" id="p1">Remember your Email and Password ?</p>
            <div class="card-body d-flex flex-row">
                <form class="col-12" @submit.prevent="login">
                <div class="form-group">
                    <label for="email">Email</label>
                    <input id="email"
                    type="email"
                    class="form-control"
                    v-model="email" required />
                </div>
                <div class="form-group">
                    <label for="caption">Password</label>
                    <input type="password"
                    class="form-control"
                    id="password"
                    v-model="password" required />
                </div>
                <div class="text-center">
                    <button type="submit" class="btn btn-info">Submit</button>
                </div>
                </form>
            </div>
            </div>
          <b-button variant="primary" block @click="hide">Close</b-button>
      </template>
    </b-sidebar>
  </div>
</template>

<script>
import jwt from 'jsonwebtoken'
export default {
  name: 'LoginButton',
  data () {
    return {
      email: '',
      password: ''
    }
  },
  methods: {
    login () {
      this.$store.dispatch('login', {
        email: this.email,
        password: this.password
      })
        .then(({ data }) => {
          const decoded = jwt.verify(data.access_token, 'rahasia')
          this.$swal('WELCOME', 'Login Succesfull', 'success')
          localStorage.setItem('access_token', data.access_token)
          this.$store.commit('SET_USERNAME', decoded.email)
          this.$store.commit('SET_LOGIN', { loggedIn: true })
        })
        .catch(err => {
          this.$swal(err.response.data.msg)
        })
    }
  }
}
</script>

<style>
body {
  font-family: Helvetica Neue, Arial, sans-serif;
  font-size: 14px;
  color: #444;
}

.btn {
  display: inline-block;
  margin-bottom: 0;
  font-weight: 400;
  text-align: center;
  vertical-align: middle;
  -ms-touch-action: manipulation;
  touch-action: manipulation;
  background-color: #42b983;
  cursor: pointer;
  color: #fff;
  border: 1px solid transparent;
  white-space: nowrap;
  padding: 6px 12px;
  font-size: 14px;
  line-height: 1.42857;
  border-radius: 4px;
}

#p1{
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}
</style>
