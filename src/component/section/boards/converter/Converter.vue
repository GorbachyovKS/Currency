<template>
  <div class="converter">
    <div class="title">
      <h3>Converter</h3>
    </div>
    <div class="fromTo">
      <div class="fromTo__title">
        <p>From</p>
        <p>To</p>
      </div>
      <div class="selectedCur">
        <div class="selectedCur__item">
          <select name="" id="" v-model="selectFrom">
            <option
              v-for="curVal in curr"
              :key="curVal.id"
              :value="curVal.Cur_Abbreviation"
            >
              {{ curVal.Cur_Abbreviation }}
            </option>
          </select>
        </div>
        <span><i class="fa-solid fa-rotate-right" @click="convert"></i></span>
        <div class="selectedCur__item">
          <select name="" id="" v-model="selectTo">
            <option
              v-for="curVal in curr"
              :key="curVal.id"
              :value="curVal.Cur_Abbreviation"
            >
              {{ curVal.Cur_Abbreviation }}
            </option>
          </select>
        </div>
      </div>
    </div>
    <div class="amount">
      <div class="amount__title">Amount</div>
      <input type="number" v-model="amount" />
    </div>
    <div class="convertredTo">
      <div class="convertredTo__title">Converted to</div>
      <input type="number" v-model="convertredTo" disabled />
    </div>
    <div class="footer">
      <div class="date">
        {{ date }}
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      curr: [],
      selectFrom: "USD",
      selectTo: "EUR",
      amount: 1,
      convertredTo: null,
      date: "",
    };
  },
  watch: {
    amount() {
      this.convertredTo = this.forConvertedTo;
    },
    selectFrom() {
      this.convertredTo = this.forConvertedTo;
    },
    selectTo() {
      this.convertredTo = this.forConvertedTo;
    },
  },
  computed: {
    forConvertedTo() {
      let cur1 = this.curr.filter(
        (item) => item.Cur_Abbreviation === this.selectFrom
      )[0];
      let cur2 = this.curr.filter(
        (item) => item.Cur_Abbreviation === this.selectTo
      )[0];
      return (
        Math.round(
          ((this.amount * cur1.Cur_OfficialRate) /
            cur1.Cur_Scale /
            (cur2.Cur_OfficialRate / cur2.Cur_Scale)) *
            100
        ) / 100
      );
    },
  },
  methods: {
    async fetchCurr() {
      try {
        const response = await axios.get(
          "https://www.nbrb.by/api/exrates/rates?periodicity=0"
        );
        this.curr = response.data;
        this.date = (new Date(response.data[0].Date) + "").slice(4, 15);
        let cur1 = this.curr.filter(
          (item) => item.Cur_Abbreviation === this.selectFrom
        )[0];
        let cur2 = this.curr.filter(
          (item) => item.Cur_Abbreviation === this.selectTo
        )[0];
        this.convertredTo =
          Math.round(
            (cur1.Cur_OfficialRate /
              cur1.Cur_Scale /
              (cur2.Cur_OfficialRate / cur2.Cur_Scale)) *
              100
          ) / 100;
      } catch (e) {
        console.log(e);
      }
    },
    convert() {
      let elem = this.selectFrom;
      this.selectFrom = this.selectTo;
      this.selectTo = elem;

      elem = this.amount;
      this.amount = this.convertredTo;
      this.convertredTo = elem;
    },
  },
  mounted() {
    this.fetchCurr();
  },
};
</script>

<style scoped>
.converter {
  display: flex;
  flex-direction: column;
  background-color: white;
  border-radius: 10px;
  padding: 20px;
  /* width: 50%; */
  flex-grow: 0.5;
  font-family: inherit;
  font-size: 12px;
}

.title,
.fa-solid {
  font-size: 1rem;
  color: #3e2b88;
}

.fa-solid:hover {
  cursor: pointer;
}

.fromTo__title,
.selectedCur {
  display: flex;
  justify-content: space-between;
  gap: 10px;
  align-items: flex-end;
  align-items: center;
}

.fromTo__title {
  margin-bottom: 5px;
}

.fromTo__title p {
  width: 28%;
}

.selectedCur__item {
  padding: 10px 20px;
  border-radius: 15px;
  background-color: #f1edff;
}

.convertredTo {
  margin-bottom: 10px;
}

.amount__title,
.convertredTo__title {
  margin-bottom: 5px;
}

input,
select {
  background-color: #f1edff;
  border: none;
  outline: none;
}

select:hover {
  cursor: pointer;
}

input {
  padding: 10px 20px;
  border-radius: 15px;
  width: 100%;
}

input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

.footer {
  display: flex;
  justify-content: flex-end;
}
</style>