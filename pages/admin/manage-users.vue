<template>
  <div :class="$style.adminContainer">
    <AdminNav />

    <!-- Onboarding Queue Section -->
    <div :class="$style.onboardingQueue">
      <h2>Onboarding Applications</h2>
      <div :class="$style.filters">
        <input
          type="text"
          v-model="searchQuery"
          placeholder="Search by name or mobile number"
        />
        <select v-model="statusFilter">
          <option value="">All Status</option>
          <option value="Pending">Verification Pending</option>
          <option value="Active">Active</option>
          <option value="Rejected">Rejected</option>
        </select>
      </div>
      <table>
        <thead>
          <tr>
            <th @click="sortBy('name')">
              Name <span v-if="sortKey === 'name'">{{ sortOrderSymbol }}</span>
            </th>
            <th @click="sortBy('mobile')">
              Mobile Number
              <span v-if="sortKey === 'mobile'">{{ sortOrderSymbol }}</span>
            </th>
            <th @click="sortBy('status')">
              Status
              <span v-if="sortKey === 'status'">{{ sortOrderSymbol }}</span>
            </th>
            <th @click="sortBy('submissionDate')">
              Submission Date
              <span v-if="sortKey === 'submissionDate'">{{
                sortOrderSymbol
              }}</span>
            </th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="partner in filteredPartners"
            :key="partner.id"
          >
            <td>{{ partner.name }}</td>
            <td>{{ partner.mobile }}</td>
            <td :class="[$style.statusBadge, statusClass(partner.status)]">
              {{ partner.status }}
            </td>
            <td>{{ partner.submissionDate }}</td>
            <td>
              <button
                :class="$style.reviewBtn"
                @click="openDocumentReview(partner)"
              >
                Review
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>

    <!-- Document Review Modal -->
    <div
      v-if="showDocumentModal"
      :class="$style.modalOverlay"
      @click.self="closeDocumentModal"
    >
      <div :class="$style.modal">
        <h5>Verify Delivery Partner</h5>
        <div :class="$style.documentReview">
          <p><strong>Name:</strong> {{ selectedPartner.name }}</p>
          <p><strong>Mobile Number:</strong> {{ selectedPartner.mobile }}</p>

          <p><strong>Documents:</strong></p>
          <ul>
            <li
              v-for="doc in selectedPartner.documents"
              :key="doc.id"
            >
              <a
                :href="doc.url"
                target="_blank"
                >{{ doc.name }}</a
              >
            </li>
          </ul>

          <!-- Approval and Rejection Actions -->
          <div :class="$style.actions">
            <button @click="approvePartner(selectedPartner.id)">Approve</button>
            <button @click="rejectPartner(selectedPartner.id)">Reject</button>
          </div>

          <!-- Input for rejection reason -->
          <div v-if="rejectionReasonRequired">
            <textarea
              v-model="rejectionReason"
              placeholder="Reason for rejection..."
            ></textarea>
            <button
              :class="$style.reviewBtn"
              @click="submitRejectionReason"
            >
              Submit Rejection
            </button>
          </div>

          <button
            :class="$style.close"
            @click="closeDocumentModal"
          >
            Close
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import AdminNav from '../../../components/Admin/AdminNavigation.vue';

export default {
  components: {
    AdminNav
  },
  data() {
    return {
      searchQuery: '',
      statusFilter: '',
      sortKey: 'name',
      sortOrder: 1, // 1 for ascending, -1 for descending
      showDocumentModal: false,
      rejectionReasonRequired: false,
      selectedPartner: null,
      rejectionReason: '',
      partners: [
        // Sample data
        {
          id: 1,
          name: 'John Doe',
          mobile: '1234567890',
          status: 'Pending',
          submissionDate: '2024-09-10',
          documents: [
            { id: 1, name: 'ID Proof', url: '/path/to/id-proof.pdf' },
            { id: 2, name: 'Address Proof', url: '/path/to/address-proof.pdf' }
          ]
        },
        {
          id: 2,
          name: 'Jack Doe',
          mobile: '1889567890',
          status: 'Active',
          submissionDate: '2024-09-10',
          documents: [
            { id: 1, name: 'ID Proof', url: '/path/to/id-proof.pdf' },
            { id: 2, name: 'Address Proof', url: '/path/to/address-proof.pdf' }
          ]
        },
        {
          id: 3,
          name: 'Will',
          mobile: '1236789890',
          status: 'Rejected',
          submissionDate: '2024-09-10',
          documents: [
            { id: 1, name: 'ID Proof', url: '/path/to/id-proof.pdf' },
            { id: 2, name: 'Address Proof', url: '/path/to/address-proof.pdf' }
          ]
        }
      ]
    };
  },
  computed: {
    filteredPartners() {
      let filtered = this.partners.filter((partner) => {
        return (
          partner.name.toLowerCase().includes(this.searchQuery.toLowerCase()) ||
          partner.mobile.includes(this.searchQuery)
        );
      });

      if (this.statusFilter) {
        filtered = filtered.filter(
          (partner) => partner.status === this.statusFilter
        );
      }

      return filtered.sort((a, b) => {
        let result = 0;
        const fieldA = (a[this.sortKey] || '').toString().toLowerCase();
        const fieldB = (b[this.sortKey] || '').toString().toLowerCase();

        if (fieldA < fieldB) result = -1;
        if (fieldA > fieldB) result = 1;
        return result * this.sortOrder;
      });
    },
    sortOrderSymbol() {
      return this.sortOrder === 1 ? '▲' : '▼'; // ▲ for ascending, ▼ for descending
    }
  },
  methods: {
    sortBy(key) {
      if (this.sortKey === key) {
        this.sortOrder = -this.sortOrder;
      } else {
        this.sortKey = key;
        this.sortOrder = 1;
      }
    },

    openDocumentReview(partner) {
      this.selectedPartner = partner;
      this.rejectionReasonRequired = false;
      this.showDocumentModal = true;
    },

    closeDocumentModal() {
      this.showDocumentModal = false;
      this.selectedPartner = null;
    },

    approvePartner(id) {
      const partner = this.partners.find((p) => p.id === id);
      partner.status = 'Active';
      this.notifyPartner(partner, 'approved');
      this.closeDocumentModal();
    },

    rejectPartner(id) {
      this.rejectionReasonRequired = true;
    },

    submitRejectionReason() {
      const partner = this.partners.find(
        (p) => p.id === this.selectedPartner.id
      );
      partner.status = 'Rejected';
      this.notifyPartner(partner, 'rejected');
      this.closeDocumentModal();
    },

    notifyPartner(partner, status) {
      const message =
        status === 'approved'
          ? `Your onboarding application has been approved. You can now start accepting orders.`
          : `Your onboarding application has been rejected. Reason: ${this.rejectionReason}. Please correct the issue and resubmit.`;
      alert(`Notification sent to ${partner.name}: ${message}`);
    },

    statusClass(status) {
      if (status === 'Active') return this.$style.textGreen;
      if (status === 'Rejected') return this.$style.textRed;
      if (status === 'Pending') return this.$style.textOrange;
    }
  }
};
</script>

<style lang="scss" module>
@import '/assets/manage-users.module.scss';
</style>
