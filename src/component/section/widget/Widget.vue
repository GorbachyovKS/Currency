<template>
  <div class="layer">
    <div class="exit" @click="deleteWidget(wg)">
      <i class="fa-solid fa-xmark"></i>
    </div>
    <div class="widget">
      <div class="widget-info">
        <div class="item-top item">
          <span class="titleCur"
            >{{ wg.cur1.Cur_Abbreviation }}/{{ wg.cur2.Cur_Abbreviation }}</span
          >
          <div :class="{ backRed: Number.isFinite(rated) }">
            <span>{{ rated }}</span
            ><span>({{ percentRated }})</span>
          </div>
        </div>
        <div class="item-bottom item">
          <span
            ><i class="fa-solid fa-arrow-right"></i>
            {{ nowRate }}
            {{ wg.cur2.Cur_Abbreviation }}</span
          >
          <span
            ><i class="fa-solid fa-arrow-left"></i>
            {{ prevRate }}
            {{ wg.cur2.Cur_Abbreviation }}</span
          >
        </div>
      </div>
      <div class="widget-chart">
        <LineChart :dataCur="AttitudeCur" v-if="!!AttitudeCur[0]" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import LineChart from "./LineChart.vue";
export default {
  components: { LineChart },
  props: {
    wg: Object,
  },
  data() {
    return {
      prevCur1: {},
      prevCur2: {},
      monthCur1: [],
      monthCur2: [],
    };
  },
  mounted() {
    this.fetchPrevCur(this.wg.cur1, 1);
    this.fetchPrevCur(this.wg.cur2);
    this.fetchMonthCur(this.wg.cur1, 1);
    this.fetchMonthCur(this.wg.cur2);
  },
  computed: {
    AttitudeCur() {
      const result = [];
      const max = Math.max(this.monthCur1.length, this.monthCur2.length);
      for (let i = 0; i < max; i++) {
        result.push(
          Math.round((this.monthCur1[i] / this.monthCur2[i]) * 10000) / 10000
        );
      }
      return result;
    },
    rated() {
      const cur1 = this.curOfScale(this.wg.cur1);
      const cur2 = this.curOfScale(this.wg.cur2);
      const prevcur1 = this.curOfScale(this.prevCur1);
      const prevcur2 = this.curOfScale(this.prevCur2);
      const result =
        Math.round((cur1 / cur2 - prevcur1 / prevcur2) * 10000) / 10000;
      if (result > 0) {
        return "+" + result;
      } else return result;
    },
    percentRated() {
      const cur1 = this.curOfScale(this.wg.cur1);
      const cur2 = this.curOfScale(this.wg.cur2);
      const prevcur1 = this.curOfScale(this.prevCur1);
      const prevcur2 = this.curOfScale(this.prevCur2);
      const result =
        Math.round(
          ((cur1 / cur2 / (prevcur1 / prevcur2)) * 100 - 100) * 10000
        ) / 10000;
      if (result > 0) {
        return "+" + result + "%";
      } else return result + "%";
    },
    nowRate() {
      const cur1 = this.curOfScale(this.wg.cur1);
      const cur2 = this.curOfScale(this.wg.cur2);
      return Math.round((cur1 / cur2) * 10000) / 10000;
    },
    prevRate() {
      const cur1 = this.curOfScale(this.prevCur1);
      const cur2 = this.curOfScale(this.prevCur2);
      return Math.round((cur1 / cur2) * 10000) / 10000;
    },
  },
  methods: {
    curOfScale(cur) {
      return cur.Cur_OfficialRate / cur.Cur_Scale;
    },
    async fetchPrevCur(cur, id) {
      try {
        const prevDay = new Date(new Date(cur.Date) - 86400000);
        const response = await axios.get(
          `https://www.nbrb.by/api/exrates/rates/${cur.Cur_ID}`,
          {
            params: {
              ondate: prevDay,
            },
          }
        );
        if (id) {
          this.prevCur1 = response.data;
        } else this.prevCur2 = response.data;
      } catch (e) {
        console.log(e);
      }
    },
    async fetchMonthCur(cur, id) {
      const date = new Date(cur.Date);
      let month = new Date(date.setMonth(date.getMonth() - 1));
      month = month.toLocaleDateString().split(".").reverse().join("-");
      try {
        const response = await axios.get(
          `https://www.nbrb.by/API/ExRates/Rates/Dynamics/${cur.Cur_ID}`,
          {
            params: {
              startdate: month,
              enddate: cur.Date.slice(0, 10),
            },
          }
        );
        if (id) {
          this.monthCur1 = [];
          response.data.forEach((item) => {
            this.monthCur1.push(item.Cur_OfficialRate);
          });
        } else {
          this.monthCur2 = [];
          response.data.forEach((item) => {
            this.monthCur2.push(item.Cur_OfficialRate);
          });
        }
      } catch (e) {
        console.log(e);
      }
    },
    deleteWidget(e) {
      this.$emit("deleteWidget", e);
    },
  },
};
</script>

<style scoped>
.layer {
  position: relative;
  transition: all 0.2s linear;
  border-radius: 10px;
}
.widget {
  background-color: #5840bb;
  border-radius: 10px;
  color: white;
  font-size: 0.8rem;
  transition: all 0.2s linear;
}

.layer:hover {
  transform: translateY(-5px);
  box-shadow: 0px 7px 30px -10px black;
}

.layer:hover .exit {
  display: block;
}

.widget-info {
  padding: 20px 15px;
  display: flex;
  justify-content: space-between;
  width: 270px;
}

.item {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.titleCur {
  text-decoration: underline;
}

.item-top div:last-child {
  color: lightgreen;
  font-size: 0.65rem;
}

.backRed {
  color: lightcoral !important;
}

.exit {
  position: absolute;
  top: 5px;
  right: 7px;
  color: white;
  display: none;
}

.exit:hover {
  cursor: pointer;
}
.exit:hover + .widget {
  background-color: rgba(250, 109, 109, 0.937);
}
</style>