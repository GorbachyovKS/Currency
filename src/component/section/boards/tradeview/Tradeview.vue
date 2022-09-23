<template>
  <div class="tradeview">
    <div class="title">
      <h3>Tradeview</h3>
      <span v-if="selectedWidget.cur1"
        >{{ selectedWidget.cur1.Cur_Abbreviation }}/{{
          selectedWidget.cur2.Cur_Abbreviation
        }}
        -</span
      >
      <span v-if="!selectedWidget.cur1">Select Widget</span>
      <select v-model="selectDate" class="selectDatetime">
        <option>Week</option>
        <option>Month</option>
        <option>Year</option>
      </select>
    </div>
    <div class="charts">
      <LineChart
        v-if="dataDate.length && !!AttitudeCur[AttitudeCur.length - 1]"
        :dataDate="dataDate"
        :dataCur="AttitudeCur"
        :rated="rated"
        :percentRate="percentRate"
      />
      <div class="load" v-if="cur1 && !dataDate.length">loading...</div>
      <div class="load" v-if="!cur1">Currency not selected</div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import LineChart from "./charts/LineChart.vue";
export default {
  components: { LineChart },
  props: {
    selectedWidget: Object,
  },
  data() {
    return {
      cur1: "",
      cur2: "",
      scale1: "",
      scale2: "",
      dataDate: [],
      selectDate: "Week",
    };
  },
  computed: {
    AttitudeCur() {
      const result = [];
      const max = Math.max(this.cur1.length, this.cur2.length);
      for (let i = 0; i < max; i++) {
        result.push(
          Math.round(
            (this.cur1[i] / this.scale1 / (this.cur2[i] / this.scale1)) * 10000
          ) / 10000
        );
      }
      return result;
    },
    rated() {
      const result = [0];
      const max = Math.max(this.cur1.length, this.cur2.length);
      for (let i = 1; i < max; i++) {
        result.push(
          Math.round((this.AttitudeCur[i] - this.AttitudeCur[i - 1]) * 10000) /
            10000
        );
      }
      return result;
    },
    percentRate() {
      const result = [0];
      const max = Math.max(this.cur1.length, this.cur2.length);
      for (let i = 1; i < max; i++) {
        result.push(
          Math.round(
            ((this.AttitudeCur[i] / this.AttitudeCur[i - 1]) * 100 - 100) *
              10000
          ) / 10000
        );
      }
      return result;
    },
  },
  watch: {
    selectDate(n) {
      if (this.selectedWidget.id) {
        this.dataDate = [];
        this.fetchMonthCur(this.selectedWidget.cur1, n, 1);
        this.fetchMonthCur(this.selectedWidget.cur2, n);
      }
    },
    selectedWidget: {
      handler(n) {
        this.dataDate = [];
        const { cur1, cur2 } = n;
        this.fetchMonthCur(cur1, this.selectDate, 1);
        this.fetchMonthCur(cur2, this.selectDate);
      },
      deep: true,
    },
  },
  methods: {
    async fetchMonthCur(cur, dateName, id) {
      const date = new Date(cur.Date);
      let monthSelect;
      if (dateName === "Week") {
        monthSelect = new Date(date.setTime(date.getTime() - 86400000 * 7));
      } else if (dateName === "Month") {
        monthSelect = new Date(date.setMonth(date.getMonth() - 1));
      } else {
        monthSelect = new Date(date.setTime(date.getTime() - 86400000 * 365));
      }
      monthSelect = monthSelect
        .toLocaleDateString()
        .split(".")
        .reverse()
        .join("-");
      try {
        const response = await axios.get(
          `https://www.nbrb.by/API/ExRates/Rates/Dynamics/${cur.Cur_ID}`,
          {
            params: {
              startdate: monthSelect,
              enddate: cur.Date.slice(0, 10),
            },
          }
        );
        if (id) {
          this.cur1 = [];
          response.data.forEach((item) => {
            this.cur1.push(item.Cur_OfficialRate);
          });
        } else {
          response.data.forEach((item) => {
            let date = new Date(item.Date) + "";
            this.dataDate.push(date.slice(4, 10));
          });
          this.cur2 = [];
          response.data.forEach((item) => {
            this.cur2.push(item.Cur_OfficialRate);
          });
        }
        this.scale1 = this.selectedWidget.cur1.Cur_Scale;
        this.scale2 = this.selectedWidget.cur2.Cur_Scale;
      } catch (e) {
        console.log(e);
      }
    },
  },
};
</script>

<style scoped>
.tradeview {
  display: flex;
  flex-direction: column;
  background-color: white;
  border-radius: 10px;
  padding: 20px;
  /* width: 50%; */
  flex-grow: 1;
}

.title {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 20px;
  padding: 0 10px 20px;
  color: #3e2b88;
}

.charts {
  height: max-content;
}

.load {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 150px;
}

.selectDatetime {
  border: none;
  font: inherit;
}
</style>