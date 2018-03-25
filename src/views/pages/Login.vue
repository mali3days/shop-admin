<template>
  <div class="app flex-row align-items-center">
    <div class="container">
      <b-row class="justify-content-center">
        <b-col md="5">
          <b-card-group>
            <b-card no-body class="p-4">
              <b-card-body>
                <h1>Login</h1>
                <p class="text-muted">Sign In to your account</p>
                <div class="input-group mb-3">
                  <span class="input-group-addon"><i class="icon-user"></i></span>
                  <input type="text" class="form-control" placeholder="Username" v-model="un">
                </div>
                <div class="input-group mb-4">
                  <span class="input-group-addon"><i class="icon-lock"></i></span>
                  <input type="password" class="form-control" placeholder="Password"  v-model="pd">
                </div>
                <b-row>
                  <b-col cols="6">
                    <b-button variant="primary" class="px-4" @click="login">Login</b-button>
                  </b-col>
                  <!-- <b-col cols="6" class="text-right">
                    <b-button variant="link" class="px-0">Forgot password?</b-button>
                  </b-col> -->
                </b-row>
              </b-card-body>
            </b-card>
            <!-- <b-card no-body class="text-white bg-primary py-5 d-md-down-none" style="width:44%">
              <b-card-body class="text-center">
                <div>
                  <h2>Sign up</h2>
                  <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.</p>
                  <b-button variant="primary" class="active mt-3">Register Now!</b-button>
                </div>
              </b-card-body>
            </b-card> -->
          </b-card-group>
        </b-col>
      </b-row>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

const linkAuth = `${process.env.ENDPOINT}/api/auth`

export default {
  name: 'Login',
  data () {
    return {
      un: '',
      pd: ''
    }
  },
  methods: {
    login () {
      const creds = { username: this.un, password: this.pd }
      console.log('!!!')
      console.log(creds)
      axios.post(`${linkAuth}/login`, creds)
        .then((response) => {
          const { data } = response
          console.log(response)
          if (data && data.token) {
            localStorage.setItem('tokenShop', JSON.stringify(data.token))
            alert('Вы залогинены в систему')
            this.$router.push('/orders')
          }
        })
        .catch(function (error) {
          console.log(error)
          localStorage.removeItem('tokenShop')
          alert('Не правильно введен Username или Password, попробйте еще раз')
        })
    }
  }
}
</script>
