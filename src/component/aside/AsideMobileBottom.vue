<template>
  <div class="asideMobile-bottom">
    <menu class="list">
      <li
        v-for="li in menu"
        :key="li.id"
        @click="activatedLi(li.id)"
        :class="{ active: li.active }"
      >
        <i class="fa-solid" :class="li.classFa"></i>
        <a :id="li.name + li.id" :href="'#' + li.name">{{
          li.active ? li.name : ""
        }}</a>
      </li>
    </menu>
  </div>
</template>

<script>
export default {
  data() {
    return {
      menu: [
        {
          id: 1,
          name: "Dashboard",
          classFa: "fa-table-columns",
          active: true,
        },
        { id: 2, name: "Converter", classFa: "fa-rotate-right", active: false },
        { id: 3, name: "Calculator", classFa: "fa-calculator", active: false },
        {
          id: 4,
          name: "Rates",
          classFa: "fa-money-bill-trend-up",
          active: false,
        },
        { id: 5, name: "Tradeview", classFa: "fa-chart-simple", active: false },
      ],
    };
  },
  methods: {
    activatedLi(e) {
      this.menu.forEach((item) => {
        if (item.id === e) {
          item.active = true;
          this.$emit("setSubtitle", item.name);
          let tag = document.getElementById(`${item.name}${item.id}`).click();
        } else item.active = false;
      });
    },
  },
};
</script>

<style scoped>
.asideMobile-bottom {
  position: -webkit-sticky; /* Safari */
  position: sticky;
  bottom: 0;
  z-index: 1;
  background-color: #5840bb;
  padding: 5px;
}

.list {
  display: flex;
  color: #8773dd;
  gap: 20px;
  justify-content: space-around;
}

.list li {
  margin: 10px 0;
  padding: 10px 15px;
  cursor: pointer;
  display: flex;
  gap: 10px;
  align-items: center;
  font-size: 1.3rem;
}

.list li a {
  color: #5840bb;
  font-size: 12px;
}

.active {
  background-color: white;
  border-radius: 20px;
}
</style>