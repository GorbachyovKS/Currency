<template>
  <div class="asideMobile-top">
    <div class="aside__title">
      {{ subtitle }}
    </div>
    <div class="aside__input">
      <div class="inputSearch" @click="$refs.inpFocused.focus()">
        <i class="fa-solid fa-magnifying-glass"></i>
        <input ref="inpFocused" type="text" placeholder="Search..." />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    subtitle: String,
  },
};
</script>

<style scoped>
.asideMobile-top {
  position: -webkit-sticky; /* Safari */
  position: sticky;
  top: 0;
  z-index: 1;
  background-color: #5840bb;
  padding: 5px;
  display: flex;
  flex-direction: column;
  gap: 10px;
  justify-content: center;
  align-items: center;
}

.aside__title {
  margin-top: 10px;
  color: white;
}

.aside__input {
  width: 85%;
}

.inputSearch {
  border-radius: 30px;
  box-shadow: 0px 7px 10px -10px black;
  padding: 0 15px;
  margin-bottom: 10px;
  display: flex;
  align-items: center;
  gap: 15px;
  color: lightgray;
  background-color: white;
}

.inputSearch input {
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
</style>