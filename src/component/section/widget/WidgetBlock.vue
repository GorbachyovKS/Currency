<template>
  <div class="widget-block">
    <Widget
      v-for="wg in widget"
      :key="wg.id"
      :wg="wg"
      @click="widgetSelect(wg)"
    />
    <div class="widget">
      <button class="widget_button" @click="modalWidgetOpen = true">
        Add Widget
        <i class="fa-solid fa-plus"></i>
      </button>
      <teleport to="body">
        <div v-show="modalWidgetOpen" class="modal">
          <form class="modal-block" @submit.prevent>
            <span style="margin-left: 20px"
              >Select currencies to build the widget:</span
            >
            <div class="param-modal">
              <div>
                <select class="select" v-model="selectModal1" required>
                  <option v-for="currSel in curr" :key="currSel.id">
                    {{ currSel.Cur_Abbreviation }}
                  </option>
                </select>
              </div>
              <span style="font-size: 1.2rem">/</span>
              <div>
                <select class="select" v-model="selectModal2" required>
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
</template>

<script>
import axios from "axios";
import Widget from "./Widget.vue";
export default {
  components: { Widget },
  data() {
    return {
      curr: [],
      widget: [],
      modalWidgetOpen: false,
      selectModal1: "USD",
      selectModal2: "EUR",
    };
  },
  methods: {
    widgetSelect(currs) {
      this.$emit("widgetSelect", currs);
    },
    addWidget() {
      if (this.selectModal2 !== this.selectModal1) {
        let result = true;
        if (this.widget.length) {
          this.widget.forEach((item) => {
            if (
              item.cur1.Cur_Abbreviation === this.selectModal1 &&
              item.cur2.Cur_Abbreviation === this.selectModal2
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
  justify-content: space-around;
  background-color: white;
  border-radius: 10px;
  width: 250px;
  height: 250px;
  padding: 5px;
}

.param-modal {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 10px;
}

.buttons {
  display: flex;
  gap: 20px;
  align-items: flex-end;
}

.buttons button {
  background-color: #5840bb;
  font-size: 1rem;
  border: none;
  color: white;
  border-radius: 7px;
  padding: 10px 20px;
  width: 100px;
}

.select {
  display: block;
  font-size: 16px;
  font-family: sans-serif;
  font-weight: 700;
  color: #444;
  line-height: 1.3;
  padding: 0.6em 1.4em 0.5em 0.8em;
  width: 95px;
  max-width: 100%;
  box-sizing: border-box;
  margin: 0;
  border: 1px solid #aaa;
  box-shadow: 0 1px 0 1px rgba(0, 0, 0, 0.04);
  border-radius: 0.5em;
  -moz-appearance: none;
  -webkit-appearance: none;
  appearance: none;
  background-color: #fff;
  background-image: url("data:image/svg+xml;charset=US-ASCII,%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%22292.4%22%20height%3D%22292.4%22%3E%3Cpath%20fill%3D%22%23007CB2%22%20d%3D%22M287%2069.4a17.6%2017.6%200%200%200-13-5.4H18.4c-5%200-9.3%201.8-12.9%205.4A17.6%2017.6%200%200%200%200%2082.2c0%205%201.8%209.3%205.4%2012.9l128%20127.9c3.6%203.6%207.8%205.4%2012.8%205.4s9.2-1.8%2012.8-5.4L287%2095c3.5-3.5%205.4-7.8%205.4-12.8%200-5-1.9-9.2-5.5-12.8z%22%2F%3E%3C%2Fsvg%3E"),
    linear-gradient(to bottom, #ffffff 0%, #e5e5e5 100%);
  background-repeat: no-repeat, repeat;
  background-position: right 0.7em top 50%, 0 0;
  background-size: 0.65em auto, 100%;
}

.select::-ms-expand {
  display: none;
}
.select:hover {
  border-color: #888;
}
.select:focus {
  border-color: #aaa;
  box-shadow: 0 0 1px 3px rgba(59, 153, 252, 0.7);
  box-shadow: 0 0 0 3px -moz-mac-focusring;
  color: #222;
  outline: none;
}
.select option {
  font-weight: normal;
}
*[dir="rtl"] .select,
:root:lang(ar) .select,
:root:lang(iw) .select {
  background-position: left 0.7em top 50%, 0 0;
  padding: 0.6em 0.8em 0.5em 1.4em;
}
</style>