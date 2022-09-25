<template>
  <aside class="aside">
    <header class="logo">
      <figure>
        <i class="fa-solid fa-hand-holding-dollar"></i>
      </figure>
      <span>Be Wealthy</span>
    </header>
    <menu class="list">
      <li>
        <i class="fa-solid fa-table-columns"></i>
        <a href="#">Dashboard</a>
      </li>
      <li>
        <i class="fa-solid fa-rotate-right"></i>
        <a href="#">Converter</a>
      </li>
      <li>
        <i class="fa-solid fa-calculator"></i>
        <a href="#">Calculator</a>
      </li>
      <li>
        <i class="fa-solid fa-money-bill-trend-up"></i>
        <a href="#">Bank Exchange rate</a>
      </li>
      <li>
        <i class="fa-solid fa-chart-simple"></i>
        <a href="#">Tradeview</a>
      </li>
      <li>
        <i class="fa-solid fa-book-open"></i>
        <a href="#">Newsfeed</a>
      </li>
      <li>
        <i class="fa-solid fa-globe"></i>
        <a href="#">Travel Helper</a>
      </li>
      <li>
        <i class="fa-solid fa-message"></i>
        <a href="#">Support</a>
      </li>
      <li>
        <i class="fa-solid fa-chart-pie"></i>
        <a href="#">Analytic</a>
      </li>
      <li>
        <i class="fa-solid fa-sliders"></i>
        <a href="#">Setting</a>
      </li>
    </menu>
    <footer>&copy; Eastbanc Technologies, 2019</footer>
  </aside>
</template>

<script>
export default {};
</script>

<style scoped>
.aside {
  height: inherit;
  padding: 25px 0 25px 30px;
  background-color: #5840bb;
  color: white;
  border-top-right-radius: 20px;
  border-bottom-right-radius: 20px;
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
  background: white;
  transition: left 0.5s;
  border-top-left-radius: 30px;
  border-bottom-left-radius: 30px;
}

.list li:hover:before {
  left: 0;
}

.list li a {
  transition: linear 0.7s;
  position: relative;
  color: white;
}

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