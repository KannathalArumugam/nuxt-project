<template>
  <div class="order-list">
    <table>
      <thead>
        <tr>
          <th>Order ID</th>
          <th>Customer Name</th>
          <th>Pickup Location</th>
          <th>Delivery Address</th>
          <th>Status</th>
          <th>Action</th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="order in orders"
          :key="order.id"
        >
          <td>{{ order.id }}</td>
          <td>{{ order.customerName }}</td>
          <td>{{ order.pickupLocation }}</td>
          <td>{{ order.deliveryAddress }}</td>
          <td>
            <span :class="`status-badge ${order.status.toLowerCase()}`">
              {{ order.status }}
            </span>
          </td>
          <td>
            <button
              class="reallocate-btn"
              @click="$emit('reallocate', order)"
            >
              Reallocate
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  props: {
    orders: {
      type: Array,
      required: true
    }
  }
};
</script>

<style lang="scss" scoped>
@import '/assets/order-review.module.scss';
</style>
