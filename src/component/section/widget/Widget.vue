<template>
  <div class="widget">
    <div class="widget-info">
      <div class="item-top item">
        <span
          >{{ wg.cur1.Cur_Abbreviation }}/{{ wg.cur2.Cur_Abbreviation }}</span
        >
        <span>+0.0004(+0.376%)</span>
      </div>
      <div class="item-bottom item">
        <span
          ><i class="fa-solid fa-arrow-right"></i> 1.11683
          {{ wg.cur2.Cur_Abbreviation }}</span
        >
        <span
          ><i class="fa-solid fa-arrow-left"></i> 1.11700
          {{ wg.cur2.Cur_Abbreviation }}</span
        >
      </div>
    </div>
    <div class="widget-chart"></div>
  </div>
</template>

<script>
export default {
  props: {
    wg: Object,
  },
};
</script>

<style scoped>
.widget {
  background-color: #5840bb;
  border-radius: 10px;
  color: white;
  font-size: 0.8rem;
}
.widget-info {
  padding: 20px 15px;
  display: flex;
  justify-content: space-between;
  width: 270px;
  height: 130px;
}

.item {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.item-top span:first-child {
  text-decoration: underline;
}

.item-top span:last-child {
  color: lightgreen;
  font-size: 0.65rem;
}
</style>