<template>
  <div class="boards">
    <div class="boards__item">
      <Tradeview
        :selectedWidget="selectedWidget"
        class="elemTranf"
        :mobileActive="mobileActive"
      />
      <ExchangeRates class="elemTranf" />
    </div>
    <div class="boards__item">
      <Converter :date="date" :curr="curr" class="elemTranf" />
      <Calculator :date="date" :curr="curr" class="elemTranf" />
      <Newsfeed class="elemTranf" />
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
    mobileActive: Boolean,
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

<style scoped>
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

@media only screen and (max-width: 1417.6px) {
  .boards__item:last-child {
    flex-wrap: wrap;
  }
}

.elemTranf {
  transition: all 0.2s linear;
}

.elemTranf:hover {
  transform: translateY(-5px);
  box-shadow: 0px 7px 30px -10px black;
}

@media only screen and (max-width: 560px) {
  .elemTranf {
    flex-grow: 1;
  }
}
</style>