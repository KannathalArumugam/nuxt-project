<template>
  <header class="admin-header">
    <h1>Welcome, {{ adminName }}</h1>
    <button
      class="logout-btn"
      @click="redirectToLogout"
    >
      <img
        src="/assets/images/logout.png"
        alt="Logout"
      />
    </button>
  </header>
  <div class="admin-body">
    <nav class="admin-nav">
      <ul>
        <li><a href="/admin/dashboard">Dashboard</a></li>
        <li><a href="/admin/deliver-partners">Deliver Partners</a></li>
        <li><a href="/admin/manage-users">Manage Users</a></li>
        <li><a href="/admin/orders">Manage Orders</a></li>
        <li><a href="/admin/settings">Settings</a></li>
      </ul>
    </nav>
  </div>
</template>

<script>
export default {
  data() {
    return {
      adminName: 'Admin'
    };
  },
  name: 'AdminNav',
  methods: {
    redirectToLogout() {
      window.location.href = '/admin/login';
    }
  }
};
</script>

<style lang="scss" scoped>
@import '/assets/admin-nav.scss';
</style>
