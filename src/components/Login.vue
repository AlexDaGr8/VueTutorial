<template>
    <div>
        <h1 v-if="logginStatus != ''">{{logginStatus}}</h1>
        <form @submit.prevent="login" class="login">
            <h1>Sign in</h1>
            <label>Email</label>
            <input required v-model="email" type="email" placeholder="Name" />
            <label>Password</label>
            <input required v-model="password" type="password" placeholder="Password" />
            <hr>
            <button type="submit">Login</button>
            <router-link to="/register">Register</router-link>
        </form>
    </div>
</template>

<script>
export default {
    data() {
        return {
            email: "",
            password: "",
            status: ''
        }
    },
    computed: {
      logginStatus () {
          return this.$store.getters.authStatus
      }  
    },
    methods: {
        login: function() {
            let email = this.email;
            let password = this.password;
            this.$store.dispatch('login', {email, password})
                .then(() => this.$router.push('/'))
                .catch(err => this.status = err)
        }
    }
}
</script>

