<template>
  <div class="boards">
    <div class="boards__item">
      <Tradeview :selectedWidget="selectedWidget" />
      <ExchangeRates />
    </div>
    <div class="boards__item">
      <Converter :date="date" :curr="curr" />
      <Calculator :date="date" :curr="curr" />
      <Newsfeed />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Calculator from "./calculator/Calculator.vue";
import Converter from "./converter/Converter.vue";
import ExchangeRates from "./exchangeRates/ExchangeRates.vue";
import Tradeview from "./tradeview/Tradeview.vue";
import Newsfeed from "./newsfeed/Newsfeed.vue";
export default {
  props: {
    selectedWidget: Object,
  },
  components: { Tradeview, ExchangeRates, Converter, Calculator, Newsfeed },
  data() {
    return {
      curr: [],
      date: "",
    };
  },
  methods: {
    async fetchCurr() {
      try {
        const response = await axios.get(
          "https://www.nbrb.by/api/exrates/rates?periodicity=0"
        );
        this.curr = response.data;
        this.date = (new Date(response.data[0].Date) + "").slice(4, 15);
      } catch (e) {
        console.log(e);
      }
    },
  },
  mounted() {
    this.fetchCurr();
  },
};
</script>
,
    Calculator
<style>
.boards {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.boards__item {
  display: flex;
  gap: 20px;
  flex-wrap: wrap;
}

.boards__item:last-child {
  flex-wrap: nowrap;
}
</style>