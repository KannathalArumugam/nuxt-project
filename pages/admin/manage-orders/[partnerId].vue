<template>
  <div class="order-list-container">
    <div class="order-header">
      <h2>Orders for {{ deliveryPartner.name }}</h2>
      <OrderSearchFilter @search="applySearchFilter" />
    </div>

    <div class="order-table">
      <OrderList
        :orders="filteredOrders"
        @reallocate="openReallocateModal"
      />
    </div>

    <ReallocateOrder
      v-if="showModal"
      :order="selectedOrder"
      @close="closeModal"
    />
  </div>
</template>

<script>
import OrderList from '@/components/Admin/OrderList.vue';
import OrderSearchFilter from '@/components/Admin/OrderSearchFilter.vue';
import ReallocateOrder from '@/components/Admin/ReallocateOrder.vue';

export default {
  components: { OrderList, OrderSearchFilter, ReallocateOrder },
  data() {
    return {
      deliveryPartner: {}, // Partner data fetched from API
      orders: [], // Orders data fetched from API
      filteredOrders: [],
      showModal: false,
      selectedOrder: null
    };
  },
  mounted() {
    const partnerId = this.$route.params.partnerId;
    this.fetchOrders(partnerId);
  },
  methods: {
    fetchOrders(partnerId) {
      // API call to fetch orders for the specific delivery partner
      this.deliveryPartner = {
        id: partnerId,
        name: 'John Doe'
      };

      this.orders = [
        {
          id: 1001,
          customerName: 'John Smith',
          pickupLocation: 'Warehouse A',
          deliveryAddress: '123 Street',
          status: 'Assigned'
        },
        {
          id: 1002,
          customerName: 'Jane Smith',
          pickupLocation: 'Warehouse B',
          deliveryAddress: '123443434 Street',
          status: 'Delivered'
        }
        // More orders...
      ];

      this.filteredOrders = this.orders; // Initially show all orders
    },
    applySearchFilter(criteria) {
      this.filteredOrders = this.orders.filter((order) => {
        const matchesSearchTerm =
          order.id.toString().includes(criteria.searchTerm) ||
          order.customerName
            .toLowerCase()
            .includes(criteria.searchTerm.toLowerCase()) ||
          order.pickupLocation
            .toLowerCase()
            .includes(criteria.searchTerm.toLowerCase()) ||
          order.deliveryAddress
            .toLowerCase()
            .includes(criteria.searchTerm.toLowerCase());

        const matchesStatus =
          !criteria.status || order.status === criteria.status;

        return matchesSearchTerm && matchesStatus;
      });
    },
    openReallocateModal(order) {
      this.selectedOrder = order;
      this.showModal = true;
    },
    closeModal() {
      this.showModal = false;
      this.selectedOrder = null;
    }
  }
};
</script>

<style lang="scss" scoped>
@import '/assets/manage-orders.module.scss';
</style>
