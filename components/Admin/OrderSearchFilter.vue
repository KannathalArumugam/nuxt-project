<template>
  <div class="search-filter">
    <input
      v-model="searchTerm"
      placeholder="Search orders by ID, Name, PIN, or City"
      @keyup.enter="applyFilter"
    />
    <select
      v-model="status"
      @change="applyFilter"
    >
      <option value="">All Statuses</option>
      <option value="Assigned">Assigned</option>
      <option value="Picked">Picked</option>
      <option value="Delivered">Delivered</option>
      <option value="Canceled">Cancelled</option>
    </select>
    <button @click="applyFilter">Search</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchTerm: '', // Single search term
      status: '' // Status filter
    };
  },
  methods: {
    applyFilter() {
      this.$emit('search', {
        searchTerm: this.searchTerm,
        status: this.status
      });
    }
  }
};
</script>

<style lang="scss" scoped>
@import '/assets/manage-orders.module.scss';
</style>
