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
  defaults,
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
    rated: Array,
    percentRate: Array,
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
            titleColor: "black",
            titleFont: { weight: "bold", size: 14 },
            bodyFont: { weight: "bold", size: 11 },
            bodyAlign: "center",
            titleAlign: "center",
            padding: 15,
            cornerRadius: 10,
            callbacks: {
              title: (context) => {
                return this.dataCur[context[0].dataIndex];
              },
              label: (context) => {
                if (!this.rated[context.dataIndex])
                  return this.rated[context.dataIndex] + ".0000";
                return this.rated[context.dataIndex] + "";
              },
              afterBody: (context) => {
                if (!this.percentRate[context[0].dataIndex])
                  return (
                    "(" + this.percentRate[context[0].dataIndex] + ".0000%)"
                  );
                return "(" + this.percentRate[context[0].dataIndex] + "%)";
              },
              labelTextColor: (context) => {
                if (this.rated[context.dataIndex] > 0) return "green";
                else return "red";
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
    // console.log(this.dataCur);
  },
};
</script>

<style>
</style>