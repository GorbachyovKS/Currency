<template>
  <Line
    :chart-options="chartOptions"
    :chart-data="chartData"
    :chart-id="chartId"
    :dataset-id-key="datasetIdKey"
    :plugins="plugins"
    :css-classes="cssClasses"
    :styles="styles"
    :width="width"
    :height="height"
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
} from "chart.js";

ChartJS.register(LineElement, LinearScale, PointElement, CategoryScale);

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
    width: {
      type: Number,
      default: 0,
    },
    height: {
      type: Number,
      default: 0,
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
  },
  data() {
    return {
      chartData: {
        labels: ["", "", "", "", "", "", "", "", "", "", "", "", ""],
        datasets: [
          {
            data: [40, 20, 12, 20, 12, 20, 12, 20, 12],
            pointBackgroundColor: "transparent",
          },
        ],
      },
      chartOptions: {
        responsive: true,
      },
      styles: {
        backgroundColor: "white",
      },
    };
  },
};
</script>

<style>
</style>