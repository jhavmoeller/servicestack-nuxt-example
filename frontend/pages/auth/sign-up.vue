<template>
  <div id="sign-in">

    <h1>Sign Up</h1>
    <form autocomplete="off" v-on:submit="signup">

      <div>
        <div>
          <label>Username</label>
        </div>
        <div>
          <input name="username" type="text" v-model="username" required>
        </div>
      </div>

      <div>
        <div>
          <label>Password</label>
        </div>
        <div>
          <input name="pwd" type="password" v-model="password" required>
        </div>
      </div>

      <div>
        <div>
          <label>Confirm Password</label>
        </div>
        <div>
          <input name="pwd2" type="password" v-model="confirmpassword" required>
        </div>
      </div>

      <div>
        <div>
          <label>Display Name</label>
        </div>
        <div>
          <input name="displayname" type="text" v-model="displayname" required>
        </div>
      </div>

      <div>
        <div>
          <label>Email</label>
        </div>
        <div>
          <input name="email" type="email" v-model="email" required>
        </div>
      </div>

      <div v-if="error" style="color: red;">
        <p>{{error}}</p>
      </div>

      <div >
        <button type="submit">Sign Up</button>&nbsp;
      </div>

      <p>Already have an account? <nuxt-link v-bind:to="signInUrl">Sign In!</nuxt-link></p>

    </form>
  </div>
</template>

<script>
import { getQueryParams } from '~/utils/auth'
import { register, extractErrorMessage } from '~/utils/api'

export default {
  middleware: 'anonymous',
  mounted () {
    const { redirect, registered } = getQueryParams()
    this.registered = registered === 'true'
    this.redirect = decodeURIComponent(redirect || '/')
  },
  data () {
    return {
      redirect: null,
      username: null,
      password: null,
      confirmpassword: null,
      displayname: null,
      email: null,
      error: null
    }
  },
  computed: {
    signInUrl () {
      return '/auth/sign-in?redirect=' + this.redirect
    }
  },
  methods: {
    async signup (event) {
      event.preventDefault()
      this.error = null
      if (this.confirmpassword !== this.password) {
        this.error = 'Passwords don\'t match'
        return
      }
      try {
        await register(this.username, '', '', this.displayname, this.email, this.password)
        this.$router.push(this.signInUrl + '&registered=true')
      } catch (err) {
        this.error = extractErrorMessage(err, 'There was an error, unable to register with those credentials.')
      }
    }
  }
}
</script>

<style scoped lang="scss">
#sign-in {
  width: 100%;
  text-align: center;

  a { cursor: pointer; }
  input[type='checkbox'] { margin-right: 10px; }
  div {
    margin-bottom: 10px;
  }
}
</style>
