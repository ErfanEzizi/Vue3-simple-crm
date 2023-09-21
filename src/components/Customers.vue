<template>
  <div>
    <div class="header">
      <h1>Customer List</h1>
      <p>Total: {{ filteredData.length }}</p>
    </div>
    <input
      v-model="searchQuery"
      @input="handleSearch"
      placeholder="Search by name..."
      class="searchBar"
    />
    <div v-if="loading" class="loading-indicator">
      <img src="../assets/loading.gif" alt="Loading..." />
    </div>
    <customer-table
      v-else-if="!loading"
      :data="filteredData"
      :per-page="perPage"
      @page-click="handlePageClick"
    />
  </div>
</template>

<script>
import { ref, computed, onMounted } from "vue";
import axios from "axios";
import CustomerTable from "./Table.vue";

export default {
  components: {
    CustomerTable,
  },
  setup() {
    const customerData = ref([]);
    const searchQuery = ref("");
    const currentPage = ref(1);
    const perPage = 20;
    const loading = ref(true);

    const fetchData = async () => {
      try {
        const response = await axios.get(
          "https://startdeliver-mock-api.glitch.me/customer"
        );
        customerData.value = response.data;
      } catch (error) {
        console.error("Error fetching data:", error);
      } finally {
        loading.value = false;
      }
    };
    // intentionally delaied fetch time to show case loading
    onMounted(() => {
      setTimeout(() => {
        fetchData();
      }, 2000);
    });

    const filteredData = computed(() => {
      return customerData.value.filter((customer) =>
        customer.name.toLowerCase().includes(searchQuery.value.toLowerCase())
      );
    });

    const pageCount = computed(() =>
      Math.ceil(filteredData.value.length / perPage)
    );

    const handlePageClick = (page) => {
      currentPage.value = page;
    };

    const handleSearch = () => {
      currentPage.value = 1;
    };

    return {
      searchQuery,
      filteredData,
      currentPage,
      pageCount,
      handleSearch,
      handlePageClick,
      loading,
    };
  },
};
</script>

<style scoped>
.loading-indicator {
  text-align: center;
  margin-top: 20px;
}

.loading-indicator > img {
  width: 150px;
}
.header {
  display: flex;
  align-items: baseline;
  gap: 10px;
}
.searchBar {
  width: 98%;
  padding: 6px;
  border-radius: 10px;
  border: 1px solid #f3e4e4;
}

.searchBar:focus {
  outline: none !important;
  border: 3px solid #584f4f2a;
}
</style>
