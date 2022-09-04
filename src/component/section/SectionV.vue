<template>
  <section class="section">
    <div class="inputSearch">
      <i class="fa-solid fa-magnifying-glass"></i>
      <input
        type="text"
        name=""
        id=""
        placeholder="Search..."
        v-model="searched"
      />
    </div>
    <div class="content">
      <div class="widget-block">
        <Widget v-for="wg in widget" :key="wg.id" :wg="wg" />
        <div class="widget">
          <button class="widget_button" @click="modalWidgetOpen = true">
            Add Widget
            <i class="fa-solid fa-plus"></i>
          </button>
          <teleport to="body">
            <div v-show="modalWidgetOpen" class="modal">
              <form class="modal-block" @submit.prevent>
                <div class="param-modal">
                  <div>
                    <select v-model="selectModal1" required>
                      <option v-for="currSel in curr" :key="currSel.id">
                        {{ currSel.Cur_Abbreviation }}
                      </option>
                    </select>
                  </div>
                  <div>
                    <select v-model="selectModal2" required>
                      <option v-for="currSel in curr" :key="currSel.id">
                        {{ currSel.Cur_Abbreviation }}
                      </option>
                    </select>
                  </div>
                </div>
                <div class="buttons">
                  <button type="submit" @click="modalWidgetOpen = false">
                    close
                  </button>
                  <button type="submit" @click="addWidget">add</button>
                </div>
                <div class="error" v-if="selectModal1 === selectModal2">
                  Выберите две разные валюты для добавления виджета
                </div>
              </form>
            </div>
          </teleport>
        </div>
      </div>
      <div class="boards"></div>
    </div>
  </section>
</template>

<script>
import axios from "axios";
import Widget from "./widget/Widget.vue";
export default {
  components: { Widget },
  data() {
    return {
      searched: "",
      curr: [],
      widget: [],
      modalWidgetOpen: false,
      selectModal1: "USD",
      selectModal2: "EUR",
    };
  },
  methods: {
    addWidget() {
      if (this.selectModal2 !== this.selectModal1) {
        let result = true;
        if (this.widget.length) {
          this.widget.forEach((item) => {
            if (
              (item.cur1.Cur_Abbreviation === this.selectModal1,
              item.cur2.Cur_Abbreviation === this.selectModal2)
            ) {
              result = false;
            }
          });
        }
        const id = Date.now();
        const cur1 = this.curr.filter(
          (cur) => cur.Cur_Abbreviation === this.selectModal1
        );
        const cur2 = this.curr.filter(
          (cur) => cur.Cur_Abbreviation === this.selectModal2
        );
        if (result) this.widget.push({ id, cur1: cur1[0], cur2: cur2[0] });
        this.modalWidgetOpen = false;
      }
    },
    async fetchCurr() {
      try {
        const response = await axios.get(
          "https://www.nbrb.by/api/exrates/rates?periodicity=0"
        );
        const data = response.data;
        this.curr = response.data;
      } catch (e) {
        console.log(e);
      }
    },
  },
  mounted() {
    this.fetchCurr();
  },
};
</script>

<style scoped>
.section {
  padding: 25px 0 25px 30px;
}
.widget-block {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
}

.widget:last-child {
  display: flex;
  justify-content: center;
  border: 3px #cbc4e8 dashed;
  border-radius: 10px;
}

.widget_button {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 10px;
  background-color: transparent;
  border: none;
  color: #cbc4e8;
  font-size: 1.2rem;
  width: 265px;
  height: 130px;
  cursor: pointer;
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
}

.inputSearch input {
  border: none;
  outline: none;
  height: 40px;
}

input::placeholder {
  color: lightgray;
  font-size: 1rem;
}

.modal {
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.modal-block {
  display: flex;
  align-items: center;
  flex-direction: column;
  justify-content: center;
  background-color: white;
  width: 300px;
  height: 300px;
  padding: 5px;
}

.buttons {
  display: flex;
  gap: 20px;
}
</style>