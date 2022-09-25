<template>
  <div class="wrapper">
    <AsideV />
    <SectionV />
  </div>
</template>

<script>
import AsideV from "./component/aside/AsideV.vue";
import SectionV from "./component/section/SectionV.vue";
export default {
  components: {
    AsideV,
    SectionV,
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  text-decoration: none;
  list-style-type: none;
  font-family: Verdana, sans-serif;
}

html,
body {
  height: 100%;
  background-color: #f6f5fc;
}

#app {
  height: inherit;
}

.wrapper {
  display: flex;
  gap: 20px;
}
</style>
