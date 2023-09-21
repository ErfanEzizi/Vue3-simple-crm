<template>
  <div>
    <h3>Home</h3>
    <div class="max-arr-info" v-if="maxArrData">
      <div>
        <p class="grey-text">Arr</p>
        <h1>$ {{ maxArrFormatted }}</h1>
      </div>
      <div>
        <p>Seats</p>
        <h1>{{ maxArrData.seats }}</h1>
      </div>
    </div>
    <div v-show="loading" class="loading-indicator">
      <img src="../assets/loading.gif" alt="Loading..." />
    </div>
    <BarChart :chartData="chartData" :maxArrData="maxArrData" />
  </div>
</template>

<script>
import { ref, onMounted } from "vue";
import BarChart from "./BarChart.vue";
import axios from "axios";

export default {
  components: {
    BarChart,
  },
  setup() {
    const chartData = ref([]);
    const maxArrData = ref(null);
    const maxArrFormatted = ref(null);
    const loading = ref(true);

    const fetchData = async () => {
      try {
        const response = await axios.get(
          "https://startdeliver-mock-api.glitch.me/report"
        );
        chartData.value = response.data.data;

        const maxArrValue = Math.max(
          ...chartData.value.map((item) => item.arr)
        );
        const maxArrIndex = chartData.value.findIndex(
          (item) => item.arr === maxArrValue
        );
        maxArrData.value = chartData.value[maxArrIndex];

        if (maxArrValue >= 1000000) {
          maxArrFormatted.value = (maxArrValue / 1000000).toFixed(1) + "M";
        } else if (maxArrValue >= 1000) {
          maxArrFormatted.value = (maxArrValue / 1000).toFixed(1) + "k";
        } else {
          maxArrFormatted.value = maxArrValue.toString();
        }
      } catch (error) {
        console.error("Error fetching data:", error);
      } finally {
        loading.value = !loading.value;
      }
    };

    // intentionally delaied fetch time to show case loading
    onMounted(() => {
      setTimeout(() => {
        fetchData();
      }, 2000);
    });

    return {
      chartData,
      maxArrData,
      maxArrFormatted,
      loading,
    };
  },
};
</script>

<style>
p,
h1 {
  margin: 0;
}
.loading-indicator {
  text-align: center;
  margin-top: 20px;
}

.loading-indicator > img {
  width: 150px;
}
.max-arr-info {
  display: flex;
  justify-content: space-evenly;
  align-items: start;
}
</style>
