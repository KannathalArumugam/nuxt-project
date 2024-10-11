<!-- <script>
export default {
  data() {
    return {
      orders: [
        {
          id: 1,
          orderNumber: '429242424',
          pickupLocation: 2.5,
          deliveryLocation: 8,
          price: 270
        },
        {
          id: 2,
          orderNumber: '4292123456',
          pickupLocation: 2.5,
          deliveryLocation: 15,
          price: 370
        },
        {
          id: 3,
          orderNumber: '429276543',
          pickupLocation: 2.5,
          deliveryLocation: 5,
          price: 2700
        },
        {
          id: 4,
          orderNumber: '429098765',
          pickupLocation: 0.5,
          deliveryLocation: 5,
          price: 130
        }
      ]
    };
  }
};
</script>

<style lang="scss" scoped>
@import '/assets/orders-list.scss';
</style>

<template>
  <div class="orders-page">
    <div class="orders-container">
      <h2>Orders List</h2>
      <p>Below are your orders list</p>

      <div
        v-for="order in orders"
        :key="order.id"
        class="order-card"
      >
        <div class="order-info">
          <p>
            <strong>Order #: </strong>
            <a
              href="#"
              class="order-link"
              >{{ order.orderNumber }}</a
            >
          </p>
          <p>
            <strong>Pick up Location: </strong>
            {{ order.pickupLocation }} Km
          </p>
          <p>
            <strong>Delivery Location: </strong>
            {{ order.deliveryLocation }} Km
          </p>
        </div>
        <div class="order-price">
          <p>₹ {{ order.price }}</p>
          <button class="start-button">Start</button>
        </div>
      </div>

      <div class="bottom-nav">
        <button class="nav-button">Profile</button>
        <button class="nav-button active">Orders List</button>
        <button class="nav-button">More</button>
      </div>
    </div>
  </div>
</template> -->

<script>
export default {
  data() {
    return {
      orders: [
        {
          id: 1,
          orderNumber: '429242424',
          pickupLocation: 2.5,
          deliveryLocation: 8,
          price: 270
        },
        {
          id: 2,
          orderNumber: '4292123456',
          pickupLocation: 2.5,
          deliveryLocation: 15,
          price: 370
        },
        {
          id: 3,
          orderNumber: '429276543',
          pickupLocation: 2.5,
          deliveryLocation: 5,
          price: 2700
        },
        {
          id: 4,
          orderNumber: '429098765',
          pickupLocation: 0.5,
          deliveryLocation: 5,
          price: 130
        }
      ]
    };
  }
};
</script>

<style lang="scss" scoped>
@import '/assets/orders-list.scss';
</style>

<template>
  <div class="orders-list-page">
    <div class="orders-header">
      <h2>Orders List</h2>
      <p>Below are your orders list</p>
    </div>

    <div
      v-for="order in orders"
      :key="order.id"
      class="order-card"
    >
      <div class="order-details">
        <div class="order-number">
          <span>Order #: </span>
          <a
            href="#"
            class="order-link"
            >{{ order.orderNumber }}</a
          >
        </div>
        <div class="order-locations">
          <p>
            Pick up Location: <span>{{ order.pickupLocation }} Km</span>
          </p>
          <p>
            Delivery Location: <span>{{ order.deliveryLocation }} Km</span>
          </p>
        </div>
      </div>
      <div class="order-actions">
        <p class="order-price">₹ {{ order.price }}</p>
        <button class="start-button">Start</button>
      </div>
    </div>

    <div class="bottom-nav">
      <button class="nav-button">Profile</button>
      <button class="nav-button active">Orders List</button>
      <button class="nav-button">More</button>
    </div>
  </div>
</template>
