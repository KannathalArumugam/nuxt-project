<script>
export default {
  data() {
    return {
      mobileNumber: '',
      otp: '',
      otpSent: false,
      generatedOtp: '',
      isAdmin: true // assuming you want to check if it's an admin portal
    };
  },
  methods: {
    sendOtp() {
      // Validate mobile number format
      const mobilePattern = /^[0-9]{10}$/;
      if (!mobilePattern.test(this.mobileNumber)) {
        alert('Please enter a valid 10-digit mobile number.');
        return;
      }

      // Generate a random 4-digit OTP
      this.generatedOtp = Math.floor(1000 + Math.random() * 9000).toString();
      console.log('Generated OTP:', this.generatedOtp);

      // Simulate sending OTP
      console.log('Sending OTP to:', this.mobileNumber);
      this.otpSent = true;
    },
    verifyOtp() {
      // Validate OTP format
      const otpPattern = /^[0-9]{4}$/;
      if (!otpPattern.test(this.otp)) {
        alert('Please enter a valid 4-digit OTP.');
        return;
      }

      // Verify the OTP
      if (this.otp === this.generatedOtp) {
        console.log('Admin logged in with mobile number:', this.mobileNumber);
        this.$router.push('/admin/dashboard');
      } else {
        console.log('Incorrect OTP');
      }
    }
  }
};
</script>

<template>
  <div class="login-page">
    <div class="login-container">
      <img
        src="/assets/images/gintaa.jpg"
        alt="Admin Portal Logo"
        class="logo"
      />
      <h1>Welcome</h1>
      <p>
        Enter your mobile number to receive an OTP and access the admin
        dashboard.
      </p>
      <form @submit.prevent="otpSent ? verifyOtp() : sendOtp()">
        <div class="input-group">
          <input
            type="tel"
            v-model="mobileNumber"
            placeholder="Enter mobile number"
            required
            :disabled="otpSent"
            pattern="[0-9]{10}"
            title="Please enter a valid mobile number"
          />
        </div>
        <div
          class="input-group"
          v-if="otpSent"
        >
          <input
            type="text"
            v-model="otp"
            placeholder="Enter OTP"
            required
            pattern="[0-9]{4}"
            title="Please enter a valid 4-digit OTP"
          />
        </div>
        <button
          type="submit"
          class="login-button"
        >
          {{ otpSent ? 'Verify OTP' : 'Send OTP' }}
        </button>
      </form>
      <p class="admin-contact">
        Need help? Contact
        <a href="mailto:support@example.com">technical support</a>.
      </p>
    </div>
  </div>
</template>

<style lang="scss" scoped>
@import '/assets/login.scss';
</style>
