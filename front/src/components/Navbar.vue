<template>
  <div class="header clearfix">
    <nav>
      <ul class="nav nav-pills pull-right">
        <router-link tag="li" to="/login" v-if="!isAuth">
          <a>Login</a>
        </router-link>
        <router-link tag="li" to="/register" v-if="!isAuth">
          <a>Register</a>
        </router-link>
        <router-link tag="li" to="/feed" v-if="isAuth">
          <a>Products</a>
        </router-link>
        <router-link tag="li" to="/products/create" v-if="isAuth">
          <a>Create</a>
        </router-link>
        <router-link tag="li" to="/logout" v-if="isAuth">
          <a @click.prevent="logout">Logout</a>
        </router-link>
      </ul>
    </nav>
    <h3 class="text-muted">FrontBack</h3>
  </div>
</template>
<script>
  export default {
    data () {
      return {
        isAuth: null
      }
    },
    created () {
      this.isAuth = this.$auth.isAuthenticated()
      this.setAuthenticatedUser()
    },
    methods: {
      logout(evt) {
        if(confirm("Are you sure you want to log out?")) {
          axios.get('api/logout').then(response => {
            localStorage.removeItem('auth_token');

            // remove any other authenticated user data you put in local storage

            // Assuming that you set this earlier for subsequent Ajax request at some point like so:
            // axios.defaults.headers.common['Authorization'] = 'Bearer ' + auth_token ;
            delete axios.defaults.headers.common['Authorization'];

            // If using 'vue-router' redirect to login page
            this.$router.go('/login');
          })
              .catch(error => {
                // If the api request failed then you still might want to remove
                // the same data from localStorage anyways
                // perhaps this code should go in a finally method instead of then and catch
                // methods to avoid duplication.
                localStorage.removeItem('auth_token');
                delete axios.defaults.headers.common['Authorization'];
                this.$router.go('/login');
              });
        }
      },
      setAuthenticatedUser () {
        this.$http.get('api/user')
            .then(response => {
              this.$auth.setAuthenticatedUser(response.body)
              console.log(this.$auth.getAuthenticatedUser())
            })
      }
    }

  }
</script>
