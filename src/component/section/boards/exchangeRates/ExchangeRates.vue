<template>
  <div class="exchangeRate">
    <div class="header">
      <h3>Exchange Rates</h3>
      <div>
        <select name="" id="" class="selectRate">
          <option value="">USD</option>
        </select>
      </div>
    </div>
    <div class="body">
      <div class="body__title">
        <span>Symbol</span>
        <span>Name</span>
        <span>Last Price</span>
        <span>Change %</span>
      </div>
      <div class="body__content">
        <div class="criptoCur" v-for="cripto in criptoCur" :key="cripto.id">
          <span>{{ cripto.symbol }}</span>
          <span>{{ cripto.name }}</span>
          <span>{{ Math.round(cripto.priceUsd * 100) / 100 }}</span>
          <span
            :style="{
              color: cripto.changePercent24Hr < 0 ? '#f75e52' : '#8cd148',
            }"
            >{{ Math.round(cripto.changePercent24Hr * 10000) / 10000 }}</span
          >
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      criptoCur: [],
    };
  },
  methods: {
    async fetchCripto() {
      const response = await axios.get("https://api.coincap.io/v2/assets");
      this.criptoCur = response.data.data;
    },
  },
  mounted() {
    this.fetchCripto();
  },
};
</script>

<style scoped>
.exchangeRate {
  display: flex;
  flex-direction: column;
  background-color: white;
  border-radius: 10px;
  padding: 20px;
  /* width: 50%; */
  flex-grow: 1;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 20px;
  padding: 0 10px 20px;
  color: #3e2b88;
}

.body {
  padding-left: 10px;
}

.body__title {
  font-size: 12px;
  margin-bottom: 20px;
  color: #a297d5;
}

.body__content {
  display: flex;
  flex-direction: column;
  gap: 10px;
  height: 230px;
  overflow-x: hidden;
  overflow-y: auto;
}

.body__title,
.criptoCur {
  display: flex;
  flex-grow: 1;
  gap: 10px;
  font-family: inherit;
}

.body__title span,
.criptoCur span {
  width: 30%;
}

.selectRate {
  border: none;
  font: inherit;
}

::-webkit-scrollbar {
  width: 7px;
  height: 8px;
  background-color: #f1edff;
  border-radius: 9em;
}

::-webkit-scrollbar-thumb {
  background-color: rgba(128, 128, 128, 0.777);
  border-radius: 9em;
  box-shadow: inset 1px 1px 10px #f3faf7;
}

::-webkit-scrollbar-button:vertical:start:decrement,
::-webkit-scrollbar-button:vertical:end:increment,
::-webkit-scrollbar-button:horizontal:start:decrement,
::-webkit-scrollbar-button:horizontal:end:increment {
  display: none;
}
</style>