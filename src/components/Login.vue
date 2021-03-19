<template>
  <div id="login" v-bind:class="{hide: loggedin}" :style="`background-image: url(${getImgUrl(`bg-${background}.jpg`)})`">
    <form @submit.prevent="formSubmit" class="login-card">
      <div class="login-card-title">Login</div>
      <label for="i-username">Username</label>
      <input placeholder="root" type="text" v-model="username" id="i-username">
      <label for="i-password">Password</label>
      <input placeholder="password" type="password" v-model="password" id="i-password">
      <button type="submit">Login</button>
    </form>
  </div>
</template>

<script>
export default {
  data: () => {
    return {
      username: '',
      password: '',
      loggedin: false,
      background: 1
    }
  },
  methods: {
    getImgUrl: function(img) {
      return require(`@/assets/${img}`)
    },
    formSubmit: function() {
      if(this.username === 'root' && this.password === 'password')
        this.loggedin = true
    }
  }
}
</script>

<style>
#login {
  background-position: center;
  background-size: cover;
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  z-index: 10000;
  display: flex;
  justify-content: center;
  align-items: center;
  color: #FFF;
  opacity: 1;
  visibility: visible;
  transition: all 300ms ease-in-out;
}

#login.hide {
  opacity: 0;
  visibility: hidden;
}

.login-card {
  display: flex;
  flex-direction: column;
  padding: 2rem;
  background: #FFF;
  background: var(--glass);
  backdrop-filter: blur(0.3rem);
}

.login-card-title {
  font-size: 1.5rem;
  margin-bottom: 1.5rem;
}

.login-card input, .login-card label {
  margin: 0.5rem 0;
}

.login-card input {
  width: 300px;
  padding: 0.5rem;
  border-radius: 5px;
  border: none;
  outline: none;
  background: transparent;
  -webkit-box-shadow: inset -6px -8px 11px -11px rgba(12,12,12,0.34), inset 9px 9px 10px -12px rgba(12,12,12,0.84); 
  box-shadow: inset -6px -8px 11px -11px rgba(12,12,12,0.34), inset 9px 9px 10px -12px rgba(12,12,12,0.84);
  color: #FFF;
}

.login-card button {
  padding: .5rem;
  border: none;
  margin-top: 1rem;
  cursor: pointer;
  outline: none;
}
</style>