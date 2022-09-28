<template>
  <div class="asideMobile">
    <menu class="list">
      <li
        v-for="li in menu"
        :key="li.id"
        @click="activatedLi(li.id)"
        :class="{ active: active }"
      >
        <i class="fa-solid" :class="li.classFa"></i>
        <a href="#" target="_blank" v-if="li.active">{{ li.name }}</a>
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
          active: false,
        },
        { id: 2, name: "Converter", classFa: "fa-rotate-right", active: false },
        { id: 3, name: "Calculator", classFa: "fa-calculator", active: false },
        {
          id: 4,
          name: "Bank Exchange rate",
          classFa: "fa-money-bill-trend-up",
          active: false,
        },
        { id: 5, name: "Tradeview", classFa: "fa-chart-simple", active: false },
        { id: 6, name: "Newsfeed", classFa: "fa-book-open", active: false },
      ],
    };
  },
  methods: {
    activatedLi(e) {
      this.menu.forEach((item) => {
        if (item.id === e) item.active = true;
        else item.active = false;
      });
    },
  },
};
</script>

<style scoped>
.asideMobile {
  position: -webkit-sticky; /* Safari */
  position: sticky;
  bottom: 0;
  z-index: 1;
  background-color: #5840bb;
  padding: 5px;
}

.list {
  display: flex;
  color: white;
  gap: 20px;
  justify-content: space-around;
}

.list li {
  padding: 20px;
  cursor: pointer;
  display: flex;
  gap: 20px;
  align-items: center;
}

.list li a {
  color: white;
}

.fa-solid {
  font-size: 1rem;
}

.active {
}
</style>