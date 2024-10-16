<template>
  <header :class="$style.adminHeader">
    <h1>Welcome, {{ adminName }}</h1>
    <button
      :class="$style.logoutBtn"
      @click="redirectToLogout"
    >
      <img
        src="/assets/images/logout.png"
        alt="Logout"
      />
    </button>
  </header>
  <div :class="$style.adminBody">
    <nav :class="$style.adminNav">
      <ul>
        <li><nuxt-link to="/admin/dashboard">Dashboard</nuxt-link></li>
        <li>
          <nuxt-link to="/admin/deliver-partners">Deliver Partners</nuxt-link>
        </li>
        <li><nuxt-link to="/admin/manage-users">Manage Users</nuxt-link></li>
        <li><nuxt-link to="/admin/manage-orders">Manage Orders</nuxt-link></li>
        <li><nuxt-link to="/admin/settings">Settings</nuxt-link></li>
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

<style lang="scss" module>
@import '/assets/admin-nav.module.scss';
</style>
