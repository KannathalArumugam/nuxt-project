<template>
  <div class="orders-table-container">
    <h2>Delivery Partner Orders Overview</h2>
    <div class="table-wrapper">
      <table class="orders-table">
        <thead>
          <tr>
            <th>Delivery Partner</th>
            <th>Total Orders</th>
            <th>Assigned</th>
            <th>Picked</th>
            <th>Delivered</th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="partner in deliveryPartners"
            :key="partner.id"
            class="table-row"
          >
            <td>
              <nuxt-link
                :to="`/admin/manage-orders/${partner.id}`"
                class="partner-link"
              >
                {{ partner.name }}
              </nuxt-link>
            </td>
            <td>{{ partner.totalOrders }}</td>
            <td>{{ partner.orderStatuses.assigned }}</td>
            <td>{{ partner.orderStatuses.picked }}</td>
            <td>{{ partner.orderStatuses.delivered }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    deliveryPartners: {
      type: Array,
      required: true
    }
  }
};
</script>

<style lang="scss" scoped>
@import '/assets/order-overview.module.scss';
</style>
