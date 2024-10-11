<script>
export default {
  data() {
    return {
      fullName: '',
      age: '',
      mobileNumber: '',
      email: '',
      language: '',
      attachment: null
    };
  },
  methods: {
    handleFileUpload(event) {
      this.attachment = event.target.files[0];
    },
    handleOnboard() {
      // Handle Onboarding Form
      console.log(
        'Onboarding with:',
        this.fullName,
        this.age,
        this.mobileNumber,
        this.email,
        this.language,
        this.attachment
      );
    }
  }
};
</script>

<style lang="scss" scoped>
@import '/assets/onboard.scss';
</style>

<template>
  <div class="onboard-page">
    <div class="onboard-container">
      <div class="header">
        <a
          href="/"
          class="back-button"
        >
          <i class="arrow-left-icon"></i>
        </a>
        <h2>Let’s Begin</h2>
      </div>
      <form @submit.prevent="handleOnboard">
        <div class="input-group">
          <input
            type="text"
            v-model="fullName"
            placeholder="Full name"
            required
          />
        </div>
        <div class="input-group">
          <input
            type="number"
            v-model="age"
            placeholder="Enter your age"
            required
          />
        </div>
        <div class="input-group">
          <input
            type="tel"
            v-model="mobileNumber"
            placeholder="Enter mobile number"
            required
          />
        </div>
        <div class="input-group">
          <input
            type="email"
            v-model="email"
            placeholder="Enter email"
            required
          />
        </div>
        <div class="input-group">
          <input
            type="text"
            v-model="language"
            placeholder="Language known"
            required
          />
        </div>
        <div class="input-group attachment">
          <i class="attachment-icon"></i>
          <label>
            <input
              type="file"
              @change="handleFileUpload"
            />
            Add Attachment
          </label>
        </div>
        <button
          type="submit"
          class="onboard-button"
        >
          Onboard Me
        </button>
      </form>
    </div>
  </div>
</template>
