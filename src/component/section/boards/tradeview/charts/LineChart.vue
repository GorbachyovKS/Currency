<template>
  <Line
    :chart-options="chartOptions"
    :chart-data="chartData"
    :chart-id="chartId"
    :dataset-id-key="datasetIdKey"
    :plugins="plugins"
    :css-classes="cssClasses"
    :styles="styles"
    :width="300"
    :height="150"
  />
</template>

<script>
import { Line } from "vue-chartjs";
import {
  Chart as ChartJS,
  LineElement,
  LinearScale,
  PointElement,
  CategoryScale,
  Filler,
  Tooltip,
  Title,
} from "chart.js";

ChartJS.register(
  LineElement,
  LinearScale,
  PointElement,
  CategoryScale,
  Filler,
  Tooltip,
  Title
);

export default {
  components: {
    Line,
  },
  props: {
    chartId: {
      type: String,
      default: "line-chart",
    },
    datasetIdKey: {
      type: String,
      default: "label",
    },
    cssClasses: {
      default: "",
      type: String,
    },
    styles: {
      type: Object,
      default: () => {},
    },
    plugins: {
      type: Object,
      default: () => {},
    },
    dataDate: Array,
    dataCur: Array,
  },
  data() {
    return {
      chartData: {
        labels: this.dataDate,
        datasets: [
          {
            data: this.dataCur,
            pointBackgroundColor: "white",
            pointBorderColor: "rgba(88,64,187,1)",
            borderColor: "rgba(88,64,187,1)",
            borderWidth: 2,
            pointHoverBackgroundColor: "red",
            pointHoverBorderColor: "red",
          },
        ],
      },
      chartOptions: {
        responsive: true,
        scales: {
          x: {
            grid: {
              display: false,
            },
          },
        },
        plugins: {
          tooltip: {
            backgroundColor: "#f6f5fc",
            bodyColor: "black",
            bodyFont: { weight: "bold" },
            padding: 15,
            cornerRadius: 10,
            callbacks: {
              title: () => {
                return;
              },
            },
            displayColors: false,
          },
        },
      },
    };
  },
  computed: {},
  mounted() {
    console.log(this.dataCur);
  },
};
</script>

<style>
</style>