<template>
  <section class="section">
    <a name="Dashboard"></a>
    <div
      class="inputSearch"
      @click="$refs.inpFocused.focus()"
      v-if="!mobileActive"
    >
      <i class="fa-solid fa-magnifying-glass"></i>
      <input
        ref="inpFocused"
        type="text"
        name=""
        id=""
        placeholder="Search..."
        v-model="searched"
      />
    </div>
    <div class="content">
      <WidgetBlock @widgetSelect="widgetSelect" :mobileActive="mobileActive" />
      <Boards :selectedWidget="selectedWidget" :mobileActive="mobileActive" />
    </div>
  </section>
</template>

<script>
import Boards from "./boards/Boards.vue";
import WidgetBlock from "./widget/WidgetBlock.vue";
export default {
  components: { WidgetBlock, Boards },
  props: {
    mobileActive: Boolean,
  },
  data() {
    return {
      searched: "",
      selectedWidget: {},
    };
  },
  methods: {
    widgetSelect(currs) {
      this.selectedWidget = currs;
    },
  },
};
</script>

<style scoped>
.section {
  padding: 25px 20px 25px 30px;
  width: 100%;
}

.inputSearch {
  border-radius: 30px;
  box-shadow: 0px 7px 10px 0px;
  padding: 0 15px;
  margin-bottom: 30px;
  display: flex;
  align-items: center;
  gap: 15px;
  color: lightgray;
  background-color: white;
}

.inputSearch input {
  flex-grow: 1;
  border: none;
  outline: none;
  height: 40px;
  font: inherit;
  font-size: 0.9rem;
}

input::placeholder {
  color: lightgray;
  font-size: 1rem;
}

.content {
  display: flex;
  flex-direction: column;
  gap: 15px;
}
</style>