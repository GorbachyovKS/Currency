<template>
  <aside class="aside">
    <header class="logo">
      <figure>
        <i class="fa-solid fa-hand-holding-dollar"></i>
      </figure>
      <span>Be Wealthy</span>
    </header>
    <menu class="list">
      <li
        v-for="li in menu"
        :key="li.id"
        @click="activated(li.id)"
        :class="{ active: li.active }"
      >
        <i class="fa-solid" :class="li.classFa"></i>
        <a href="#">{{ li.name }}</a>
      </li>
    </menu>
    <footer>&copy; Eastbanc Technologies, 2019</footer>
  </aside>
</template>

<script>
import AsideMobile from "./AsideMobile.vue";
export default {
  components: { AsideMobile },
  props: {
  },
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
          name: "Bank Exchange rate",
          classFa: "fa-money-bill-trend-up",
          active: false,
        },
        { id: 5, name: "Tradeview", classFa: "fa-chart-simple", active: false },
        { id: 6, name: "Newsfeed", classFa: "fa-book-open", active: false },
        { id: 7, name: "Travel Helper", classFa: "fa-globe", active: false },
        { id: 8, name: "Support", classFa: "fa-message", active: false },
        { id: 9, name: "Analytic", classFa: "fa-chart-pie" },
        { id: 10, name: "Setting", classFa: "fa-sliders", active: false },
      ],
    };
  },
  methods: {
    activated(e) {
      this.menu.forEach((item) => {
        if (item.id === e) item.active = true;
        else item.active = false;
      });
    },
  },
};
</script>

<style scoped>
.aside {
  padding: 25px 0 25px 30px;
  background-color: #5840bb;
  color: white;
  border-top-right-radius: 20px;
  border-bottom-right-radius: 20px;
  height: inherit;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  gap: 20px;
}
.fa-solid {
  color: #8773dd;
  font-size: 1.3rem;
  width: 30px;
}

.logo {
  display: flex;
  align-items: center;
}

.logo figure {
  background-color: #b1a1e9;
  width: 30px;
  height: 30px;
  border-radius: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-right: 10px;
}

.logo figure i {
  width: max-content;
  color: white;
  font-size: 1rem;
}

.logo span {
  font-size: 1.2rem;
}

.list {
  font-size: 1rem;
  display: flex;
  flex-direction: column;
  gap: 20px;
  margin-bottom: 20px;
  width: max-content;
  flex-grow: 1;
}

.list li {
  display: flex;
  align-items: center;
  padding: 0 20px 0 10px;
  height: 35px;
  position: relative;
  overflow: hidden;
}

.list li:after {
  content: "";
  position: absolute;
}

.list li:before {
  content: "";
  position: absolute;
  top: 0;
  left: 100%;
  width: 100%;
  height: 100%;
  background: #f6f5fc;
  transition: left 0.5s;
  border-top-left-radius: 30px;
  border-bottom-left-radius: 30px;
}

.list .active:before,
.list li:hover:before {
  left: 0;
}

.list li a {
  transition: linear 0.7s;
  position: relative;
  color: white;
}

.list .active a,
.list li:hover a {
  color: #3e2b88;
  position: relative;
}

.list li i {
  margin-right: 10px;
  position: relative;
}

footer {
  font-size: 0.8rem;
}
</style>