<template>
  <div class="chart-container">
    <canvas ref="barChart" class="barChart"></canvas>
  </div>
</template>

<script>
import { ref, watch } from "vue";
import Chart from "chart.js/auto";
import moment from "moment";

function formatDate(inputDate) {
  const parsedDate = moment(inputDate, "YYYY-MM");
  const formattedDate = parsedDate.format("MMMM YYYY");
  return formattedDate;
}

export default {
  props: {
    chartData: Array,
    maxArrData: Object,
  },
  setup(props) {
    const barChart = ref(null);
    let chartInstance = null;

    watch([() => props.chartData, () => props.maxArrData], () => {
      if (barChart.value) {
        const ctx = barChart.value.getContext("2d");

        chartInstance = new Chart(ctx, {
          type: "bar",
          data: {
            labels: props.chartData.map((item) => formatDate(item.month)),
            datasets: [
              {
                label: "Arr",
                data: props.chartData.map((item) => item.arr),
                backgroundColor: "#2050fe",
                hoverBackgroundColor: "#2050fe",
                borderWidth: 1,
                borderRadius: 5,
              },
            ],
          },
          options: {
            responsive: false, // Disable responsiveness
            maintainAspectRatio: true,
            scales: {
              y: {
                beginAtZero: true,
                max: props.maxArrData.arr, // Use the max Arr value from props
                min: 0, // Set the minimum y-axis value
              },
            },
            plugins: {
              legend: {
                display: false, // Hide the legend
              },
            },
            onHover: (event, chartElements) => {
              barChart.value.style.cursor = chartElements[0]
                ? "pointer"
                : "default";
            },
          },
        });
      }
    });

    return {
      barChart,
    };
  },
};
</script>

<style scoped>
.chart-container {
  position: relative;
  width: inherit;
  margin: 0 auto;
}

.barChart {
  width: 100%;
}

.max-arr-info {
  text-align: center;
  margin-top: 20px;
  font-weight: bold;
}
</style>
