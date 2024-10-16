<template>
  <div
    class="modal"
    @click="$emit('close')"
  >
    <div
      class="modal-content"
      @click.stop
    >
      <h3 class="modal-title">Reallocate Order</h3>
      <p class="order-info">
        Order ID: <strong>{{ order.id }}</strong>
      </p>

      <div class="form-group">
        <label for="deliveryPartner">Reassign to:</label>
        <select
          v-model="selectedPartner"
          class="form-select"
        >
          <option
            v-for="partner in availablePartners"
            :key="partner.id"
            :value="partner.id"
          >
            {{ partner.name }}
          </option>
        </select>
      </div>

      <div class="modal-actions">
        <button
          class="btn btn-confirm"
          @click="reallocate"
        >
          Confirm
        </button>
        <button
          class="btn btn-close"
          @click="$emit('close')"
        >
          Close
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    order: Object
  },
  data() {
    return {
      selectedPartner: null,
      availablePartners: [
        { id: 1, name: 'Partner A' },
        { id: 2, name: 'Partner B' }
      ]
    };
  },
  methods: {
    reallocate() {
      // Emit reallocation event or perform API call for reallocation
      this.$emit('close');
    }
  }
};
</script>

<style lang="scss" scoped>
@import '/assets/reallocate-modal.module.scss';
</style>
