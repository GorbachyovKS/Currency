<template>
  <Line
    ref="line"
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
} from "chart.js";

ChartJS.register(LineElement, LinearScale, PointElement, CategoryScale, Filler);

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
      },
    };
  },
};
</script>

<style>
</style>