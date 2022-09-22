<template>
  <Line
    :chart-options="chartOptions"
    :chart-data="chartData"
    :chart-id="chartId"
    :dataset-id-key="datasetIdKey"
    :plugins="plugins"
    :css-classes="cssClasses"
    :styles="styles"
    :width="265"
    :height="70"
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
    dataCur: {
      Array,
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
        labels: Array.from({ length: this.dataCur.length }).map((u) => ""),
        datasets: [
          {
            data: this.dataCur,
            pointBackgroundColor: "transparent",
            pointBorderColor: "transparent",
            borderColor: "white",
            borderWidth: 1,
            backgroundColor: (ctx) => {
              const canvas = ctx.chart.ctx;
              const gradient = canvas.createLinearGradient(0, 0, 0, 160);

              gradient.addColorStop(0, "rgba(255,255,255,0.2)");
              gradient.addColorStop(1, "rgba(255,255,255,0)");

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
        plugins: {
          tooltip: {
            enabled: false,
          },
        },
      },
    };
  },
};
</script>

<style>
</style>