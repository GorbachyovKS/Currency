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
        labels: ["", "", "", "", "", "", "", ""],
        datasets: [
          {
            data: [1, 2, 2.56, 2.623, 1.3452, 3, 2, 1],
            pointBackgroundColor: "transparent",
            pointBorderColor: "transparent",
            borderColor: "white",
            borderWidth: 1,
            backgroundColor: (ctx) => {
              const canvas = ctx.chart.ctx;
              const gradient = canvas.createLinearGradient(0, 0, 0, 160);

              gradient.addColorStop(0, "rgba(255,255,255,0.7)");
              gradient.addColorStop(0.5, "rgba(255,255,255,0.1)");
              gradient.addColorStop(1, "transparent");

              return gradient;
            },
            fill: true,
          },
        ],
      },
      chartOptions: {
        responsive: true,
        scales: {
          x: {
            display: false,
          },
          y: {
            display: false,
          },
        },
      },
    };
  },
  mounted() {},
};
</script>

<style>
</style>