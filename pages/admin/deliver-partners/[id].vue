<template>
  <div class="deliver-partner-profile">
    <h2>Deliver Partner Profile</h2>
    <div v-if="partner">
      <h3>Personal Information</h3>
      <p><strong>Name:</strong> {{ partner.name }}</p>
      <p><strong>Mobile Number:</strong> {{ partner.mobile }}</p>
      <p><strong>Address:</strong> {{ partner.address }}</p>
      <!-- Additional sections like documents, service area, etc. -->
    </div>
    <div v-else>
      <p>Loading...</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      partner: null
    };
  },
  async fetch() {
    const id = this.$route.params.id;
    // Fetch partner details using the ID
    const response = await fetch(`/admin/deliver-partners/${id}`);
    this.partner = await response.json();
  }
};
</script>

<style lang="scss" scoped>
@import '/assets/deliver-partners.scss';
</style>
