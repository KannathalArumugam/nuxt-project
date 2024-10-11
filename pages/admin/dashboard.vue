<template>
  <AdminNav />
  <main class="admin-content">
    <h2>Dashboard</h2>
    <section class="dashboard-overview">
      <div class="metrics">
        <div class="metric">
          <h2>Total Deliver Partners</h2>
          <p>{{ totalPartners }}</p>
        </div>
        <div class="metric">
          <h2>Active Orders</h2>
          <p>{{ activeOrders }}</p>
        </div>
        <div class="metric">
          <h2>Pending Verifications</h2>
          <p>{{ pendingVerifications }}</p>
        </div>
        <div class="metric">
          <h2>Total Revenue</h2>
          <p>{{ totalRevenue }}</p>
        </div>
      </div>
    </section>

    <!-- Recent Activities -->
    <section class="recent-activities">
      <h2>Recent Activities</h2>
      <ul>
        <li
          v-for="activity in recentActivities"
          :key="activity.id"
        >
          {{ activity.description }}
        </li>
      </ul>
    </section>

    <!-- Quick Actions -->
    <section class="quick-actions">
      <h2>Quick Actions</h2>
      <button @click="addDeliverPartner">Add Deliver Partner</button>
      <button @click="viewPendingVerifications">
        View Pending Verifications
      </button>
      <button @click="generateReport">Generate Report</button>
    </section>
  </main>
</template>

<script>
import AdminNav from '../../components/Admin/AdminNavigation.vue';
export default {
  components: {
    AdminNav
  },
  data() {
    return {
      totalPartners: 120,
      activeOrders: 45,
      pendingVerifications: 8,
      totalRevenue: 'Rs 15,000',
      recentActivities: [
        { id: 1, description: 'New deliver partner registered: John Doe' },
        { id: 2, description: 'Order #1023 delivered successfully' },
        { id: 3, description: 'Verification pending for partner: Jane Smith' }
      ]
    };
  },
  methods: {
    addDeliverPartner() {
      // Logic to add a new deliver partner
    },
    viewPendingVerifications() {
      // Logic to view pending verifications
    },
    generateReport() {
      // Logic to generate a report
    }
  }
};
</script>

<style lang="scss" scoped>
@import '/assets/dashboard.scss';
</style>
