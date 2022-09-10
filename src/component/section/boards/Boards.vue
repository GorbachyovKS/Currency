<template>
  <div class="boards">
    <Tradeview :selectedWidget="selectedWidget" />
  </div>
</template>

<script>
import Tradeview from "./tradeview/Tradeview.vue";
export default {
  props: {
    selectedWidget: Object,
  },
  components: { Tradeview },
};
</script>

<style>
</style>