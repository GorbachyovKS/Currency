<template>
  <div class="boards">
    <div class="boards__item">
      <Tradeview :selectedWidget="selectedWidget" />
      <ExchangeRates />
    </div>
    <div class="boards__item">
      <Converter />
    </div>
  </div>
</template>

<script>
import Converter from "./converter/Converter.vue";
import ExchangeRates from "./exchangeRates/ExchangeRates.vue";
import Tradeview from "./tradeview/Tradeview.vue";
export default {
  props: {
    selectedWidget: Object,
  },
  components: { Tradeview, ExchangeRates, Converter },
};
</script>

<style>
.boards {
  display: flex;
  flex-direction: column;
  gap: 40px;
}

.boards__item {
  display: flex;
  gap: 20px;
  flex-wrap: wrap;
}
</style>