<script>
export default {
  data() {
    return {
      email: '',
      password: '',
      showPassword: false
    };
  },
  methods: {
    togglePassword() {
      this.showPassword = !this.showPassword;
    },
    handleSubmit() {
      // Handle Submit
      console.log('Logging in with:', this.email, this.password);
    }
  }
};
</script>

<template>
  <div class="login-page">
    <div class="login-container">
      <img
        src="/public/image.png"
        alt="Gintaa Logo"
        class="logo"
      />
      <h1>Welcome</h1>
      <p>Fill out the information below in order to access your account.</p>
      <form @submit.prevent="handleSubmit">
        <div class="input-group">
          <input
            type="email"
            v-model="email"
            placeholder="Email"
            required
          />
        </div>
        <div class="input-group">
          <input
            :type="showPassword ? 'text' : 'password'"
            v-model="password"
            placeholder="Password"
            required
          />
          <i
            @click="togglePassword"
            class="password-toggle"
          ></i>
        </div>
        <button
          type="submit"
          class="login-button"
        >
          Log In
        </button>
      </form>
      <a
        href="/forgot-password"
        class="forgot-password"
        >Forget password</a
      >
      <p class="sign-up">
        Don’t have an account? <a href="/signup">Onboard here</a>
      </p>
    </div>
  </div>
</template>

<style lang="scss" scoped>
@import '/assets/login.scss';
</style>
