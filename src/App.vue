<template>
  <div class="wrapper" :class="{ mobilaDisplay: mobileActive }">
    <AsideV v-if="!mobileActive" />
    <SectionV :mobileActive="mobileActive" />
    <AsideMobile v-if="mobileActive" />
  </div>
</template>

<script>
import AsideMobile from "./component/aside/AsideMobile.vue";
import AsideV from "./component/aside/AsideV.vue";
import SectionV from "./component/section/SectionV.vue";
export default {
  components: {
    AsideV,
    SectionV,
    AsideMobile,
  },
  data() {
    return {
      mobileActive: false,
    };
  },
  mounted() {
    window.onresize = (e) => {
      let size = e.target.innerWidth;
      if (size < 850) this.mobileActive = true;
      else this.mobileActive = false;
    };
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
  height: max-content;
  background-color: #f6f5fc;
}

#app {
  height: inherit;
}

.wrapper {
  display: flex;
  gap: 20px;
}

.mobilaDisplay {
  display: block;
  gap: 0px;
}
</style>
