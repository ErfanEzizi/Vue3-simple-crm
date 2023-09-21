<template>
  <div>
    <table class="customer-table">
      <thead>
        <tr>
          <th>Name</th>
          <th>Arr</th>
          <th>Seats</th>
          <th>ID</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(customer, index) in paginatedData" :key="index">
          <td>{{ customer.name }}</td>
          <td>{{ customer.arr }}</td>
          <td>{{ customer.seats }}</td>
          <td>{{ customer.id }}</td>
        </tr>
      </tbody>
    </table>
    <div class="pagination">
      <button @click="prevPage" :disabled="currentPage === 1">Prev</button>
      <span>Page {{ currentPage }} of {{ pageCount }}</span>
      <button @click="nextPage" :disabled="currentPage === pageCount">
        Next
      </button>
    </div>
  </div>
</template>

<script>
import { ref, computed, onMounted } from "vue";
import axios from "axios";

export default {
  props: {
    data: Array,
    perPage: {
      type: Number,
      default: 15,
    },
  },
  setup(props) {
    const currentPage = ref(1);

    const pageCount = computed(() =>
      Math.ceil(props.data.length / props.perPage)
    );

    const paginatedData = computed(() => {
      const start = (currentPage.value - 1) * props.perPage;
      const end = start + props.perPage;
      return props.data.slice(start, end);
    });

    const prevPage = () => {
      if (currentPage.value > 1) {
        currentPage.value--;
      }
    };

    const nextPage = () => {
      if (currentPage.value < pageCount.value) {
        currentPage.value++;
      }
    };

    return {
      currentPage,
      pageCount,
      paginatedData,
      prevPage,
      nextPage,
    };
  },
};
</script>

<style scoped>
.customer-table {
  width: 100%;
  border-collapse: separate;
  border-spacing: 0 8px;
}

.customer-table th,
.customer-table td {
  border: none;
  padding: 8px;
  text-align: left;
}

.customer-table th {
  background-color: #9baebd;
}

.customer-table tr:nth-child(odd) {
  background-color: #f9f9f9;
}

.customer-table tr:hover {
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.pagination {
  display: flex;
  align-items: center;
  justify-content: center;
}

.pagination button {
  margin: 0 5px;
  padding: 5px 10px;
  border: none;
  cursor: pointer;
}

.pagination button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}
</style>
