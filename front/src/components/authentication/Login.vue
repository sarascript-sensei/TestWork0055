<template>
  <div class="row">
    <div class="col-md-6 col-md-offset-3">
      <div class="panel panel-default">
        <div class="panel-body">
          <div class="form-group">
            <input v-model="email" type="email" class="form-control"
              placeholder="Email">
          </div>
          <div class="form-group">
            <input v-model="password" type="password" class="form-control"
              placeholder="Password">
          </div>
          <button @click="login" class="btn btn-success pull-right">
            Login
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      email    : '',
      password : '',
    }
  },
  methods: {
    login () {
      var data = {
        client_id: 2,
        client_secret: 'o2Yflmy1ncwqAYszoTYg9yulGS77u8kAfgurIZeq',
        grant_type: 'password',
        username: this.email,
        password: this.password,
      }
      this.$http.post("oauth/token", data)
          .then(response => {
            this.$auth.setToken(response.body.access_token, response.body.expires_in + Date.now())
            this.$router.push('/feed')
          }, response => {
            console.log(response);
          });
    },
  }
}
</script>

<style>
</style>
