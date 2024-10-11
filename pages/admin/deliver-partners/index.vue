<template>
  <AdminNav />
  <div class="deliver-partner-listing">
    <h2>Deliver Partners</h2>
    <div class="grouping-filters">
      <input
        type="text"
        v-model="searchQuery"
        placeholder="Search by name, mobile number, or postal code"
      />
      <select v-model="statusFilter">
        <option value="">All Status</option>
        <option value="Active">Active</option>
        <option value="Inactive">Inactive</option>
        <option value="Pending">Verification Pending</option>
      </select>
    </div>
    <table>
      <thead>
        <tr>
          <th @click="sortBy('firstName')">
            Name
            <span v-if="sortKey === 'firstName'">{{ sortOrderSymbol }}</span>
          </th>
          <th @click="sortBy('mobile')">
            Mobile Number
            <span v-if="sortKey === 'mobile'">{{ sortOrderSymbol }}</span>
          </th>
          <th @click="sortBy('status')">
            Status
            <span v-if="sortKey === 'status'">{{ sortOrderSymbol }}</span>
          </th>
          <th @click="sortBy('serviceablePincode')">
            Location
            <span v-if="sortKey === 'serviceablePincode'">{{
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
          <td>{{ partner.firstName }}</td>
          <td>{{ partner.mobile }}</td>
          <td :class="['status-badge', statusClass(partner.status)]">
            {{ partner.status }}
          </td>
          <td>{{ partner.serviceablePincode }}</td>
          <td>
            <button @click="openDetails(partner)">View Details</button>
            <button @click="toggleStatus(partner.id)">
              {{ partner.status === 'Active' ? 'Deactivate' : 'Activate' }}
            </button>
          </td>
        </tr>
      </tbody>
    </table>

    <div
      v-if="showModal"
      class="modal-overlay"
      @click.self="closeModal"
    >
      <div class="modal">
        <h5>Partner Details</h5>
        <div class="deliver-partner-details">
          <p><strong>First Name:</strong> {{ selectedPartner.firstName }}</p>
          <p><strong>Gender:</strong> {{ selectedPartner.gender }}</p>
          <p><strong>Date of Birth:</strong> {{ selectedPartner.dob }}</p>
          <p><strong>Mobile Number:</strong> {{ selectedPartner.mobile }}</p>
          <p>
            <strong>Address:</strong> {{ selectedPartner.address.street }},
            {{ selectedPartner.address.area }},
            {{ selectedPartner.address.city }},
            {{ selectedPartner.address.district }},
            {{ selectedPartner.address.pincode }}
          </p>
          <p><strong>State:</strong> {{ selectedPartner.address.state }}</p>
          <p><strong>Country:</strong> {{ selectedPartner.address.country }}</p>
          <p>
            <strong>Serviceable PIN Code:</strong>
            {{ selectedPartner.serviceablePincode }}
          </p>
          <p>
            <strong>Selected Delivery Vehicle:</strong>
            {{ selectedPartner.vehicle }}
          </p>
          <p>
            <strong>Languages:</strong>
            {{ selectedPartner.languages.join(', ') }}
          </p>
          <p><strong>Uploaded Documents:</strong></p>

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

          <!-- Profile actions -->
          <div class="profile-actions">
            <button @click="requestReupload(selectedPartner.id)">
              Request Document Re-upload
            </button>
            <button @click="toggleStatus(selectedPartner.id)">
              {{
                selectedPartner.status === 'Active'
                  ? 'Deactivate Partner'
                  : 'Activate Partner'
              }}
            </button>
          </div>

          <!-- Notes or comments -->
          <div class="admin-notes">
            <textarea
              v-model="adminNotes"
              placeholder="Add notes or comments..."
            ></textarea>
            <button @click="saveNotes(selectedPartner.id)">Save Notes</button>
          </div>

          <button @click="closeModal">Close</button>
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
      showModal: false,
      selectedPartner: null,
      adminNotes: '',
      partners: [
        // Example data

        {
          id: 1,
          firstName: 'Jack',
          gender: 'Male',
          dob: '1990-01-01',
          mobile: '1234567890',
          status: 'Active',
          address: {
            street: '123 Main St',
            area: 'Downtown',
            city: 'Bangalore',
            district: 'Bangalore Urban',
            pincode: '560001',
            state: 'Karnataka',
            country: 'India'
          },
          vehicle: 'Two Wheeler (Bike)',
          serviceablePincode: '560003',
          languages: ['English', 'Kannada', 'Tamil'],
          documents: [
            { id: 1, name: 'KYC Document', url: '/path/to/kyc.pdf' },
            { id: 2, name: 'Profile Photo', url: '/path/to/photo.jpg' }
          ]
        },
        {
          id: 2,
          firstName: 'Shreyas',
          gender: 'Male',
          dob: '2000-01-01',
          mobile: '0987654321',
          status: 'Inactive',
          address: {
            street: '123 Main St',
            area: 'Downtown',
            city: 'Bangalore',
            district: 'Bangalore Urban',
            pincode: '560001',
            state: 'Karnataka',
            country: 'India'
          },
          vehicle: 'Two Wheeler (Bike)',
          serviceablePincode: '560002',
          languages: ['English', 'Kannada'],
          documents: [
            { id: 1, name: 'KYC Document', url: '/path/to/kyc.pdf' },
            { id: 2, name: 'Profile Photo', url: '/path/to/photo.jpg' }
          ]
        },
        {
          id: 3,
          firstName: 'Smith',
          gender: 'Male',
          dob: '2000-02-01',
          mobile: '7898765435',
          status: 'Pending',
          address: {
            street: '123 Main St',
            area: 'Downtown',
            city: 'Bangalore',
            district: 'Bangalore Urban',
            pincode: '560001',
            state: 'Karnataka',
            country: 'India'
          },
          vehicle: 'Two Wheeler (Bike)',
          serviceablePincode: '560007',
          languages: ['English', 'Kannada'],
          documents: [
            { id: 1, name: 'KYC Document', url: '/path/to/kyc.pdf' },
            { id: 2, name: 'Profile Photo', url: '/path/to/photo.jpg' }
          ]
        },
        {
          id: 4,
          firstName: 'Justin',
          gender: 'Male',
          dob: '2000-02-01',
          mobile: '7564765435',
          status: 'Pending',
          address: {
            street: '123 Main St',
            area: 'Downtown',
            city: 'Bangalore',
            district: 'Bangalore Urban',
            pincode: '560001',
            state: 'Karnataka',
            country: 'India'
          },
          vehicle: 'Two Wheeler (Bike)',
          serviceablePincode: '560071',
          languages: ['English', 'Kannada'],
          documents: [
            { id: 1, name: 'KYC Document', url: '/path/to/kyc.pdf' },
            { id: 2, name: 'Profile Photo', url: '/path/to/photo.jpg' }
          ]
        }
      ]
    };
  },
  computed: {
    filteredPartners() {
      let filtered = this.partners.filter((partner) => {
        return (
          partner.firstName
            .toLowerCase()
            .includes(this.searchQuery.toLowerCase()) ||
          partner.mobile.includes(this.searchQuery) ||
          partner.serviceablePincode.includes(this.searchQuery)
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

    openDetails(partner) {
      this.selectedPartner = partner;
      this.showModal = true;
    },
    closeModal() {
      this.showModal = false;
      this.selectedPartner = null;
    },

    toggleStatus(id) {
      const partner = this.partners.find((p) => p.id === id);
      partner.status = partner.status === 'Active' ? 'Inactive' : 'Active';
      // Add logic to update status in the backend
    },

    requestReupload(id) {
      // Logic to request document re-upload
      alert('Request sent to partner to re-upload documents.');
    },
    saveNotes(id) {
      // Logic to save notes
      alert('Notes saved for partner.');
    },
    statusClass(status) {
      if (status === 'Active') return 'text-green';
      if (status === 'Inactive') return 'text-red';
      if (status === 'Pending') return 'text-orange';
    }
  }
};
</script>

<style lang="scss" scoped>
@import '/assets/deliver-partners.scss';
</style>
