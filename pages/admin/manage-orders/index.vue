<template>
  <div class="admin-container">
    <AdminNav />

    <div class="orders-overview">
      <h2>Orders Overview</h2>
      <div class="overview-card">
        <!-- Display loading or empty state if no data -->
        <div
          v-if="deliveryPartners.length === 0"
          class="loading-state"
        >
          <p>Loading Order Details...</p>
        </div>
        <div v-else>
          <OrdersOverview :deliveryPartners="deliveryPartners" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import OrdersOverview from '@/components/Admin/OrdersOverview.vue';
import AdminNav from '@/components/Admin/AdminNavigation.vue';

export default {
  components: { OrdersOverview, AdminNav },
  data() {
    return {
      deliveryPartners: [] // This will be populated with data fetched from an API
    };
  },
  mounted() {
    // Fetch the data of delivery partners and their assigned orders
    this.fetchDeliveryPartners();
  },
  methods: {
    fetchDeliveryPartners() {
      // API call to fetch the delivery partners and their assigned orders
      setTimeout(() => {
        this.deliveryPartners = [
          {
            id: 1,
            name: 'John',
            totalOrders: 5,
            orderStatuses: { assigned: 2, picked: 1, delivered: 2 }
          },
          {
            id: 2,
            name: 'Jane Smith',
            totalOrders: 3,
            orderStatuses: { assigned: 1, picked: 1, delivered: 1 }
          },
          {
            id: 3,
            name: 'Smith',
            totalOrders: 4,
            orderStatuses: { assigned: 2, picked: 1, delivered: 1 }
          }
        ];
      }, 1000); // Simulate API delay
    }
  }
};
</script>

<style lang="scss" scoped>
@import '/assets/manage-orders.module.scss';
</style>
