<template>
  <div class="wrapper" :class="{ mobilaDisplay: mobileActive }">
    <AsideV v-if="!mobileActive" />
    <AsideMobileTop v-if="mobileActive" :subtitle="subtitle" />
    <SectionV :mobileActive="mobileActive" />
    <AsideMobileBottom v-if="mobileActive" @setSubtitle="setSubtitle" />
  </div>
</template>

<script>
import AsideMobileBottom from "./component/aside/AsideMobileBottom.vue";
import AsideMobileTop from "./component/aside/AsideMobileTop.vue";
import AsideV from "./component/aside/AsideV.vue";
import SectionV from "./component/section/SectionV.vue";
export default {
  components: {
    AsideV,
    SectionV,
    AsideMobileBottom,
    AsideMobileTop,
  },
  data() {
    return {
      mobileActive: false,
      subtitle: "Dashboard",
    };
  },
  methods: {
    setSubtitle(e) {
      this.subtitle = e;
    },
  },
  mounted() {
    if (window.innerWidth < 850) this.mobileActive = true;
    window.onresize = (e) => {
      let size = e.target.innerWidth;
      if (size < 850) this.mobileActive = true;
      else this.mobileActive = false;
    };
    // window.onscroll = () => {
    //   if (this.mobileActive) {
    //     console.log(window.pageYOffset);
    //     let scroll = window.pageYOffset;
    //     if (scroll >= 530) {
    //       if (scroll >= 905) {
    //         if (scroll >= 1220) {
    //           if (scroll >= 1550) {
    //             this.subtitle = "Newsfeed";
    //           } else this.subtitle = "Calculator";
    //         } else this.subtitle = "Converter";
    //       } else this.subtitle = "Rates";
    //     } else this.subtitle = "Dashboard";
    //   }
    // };
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
  scroll-behavior: smooth;
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
