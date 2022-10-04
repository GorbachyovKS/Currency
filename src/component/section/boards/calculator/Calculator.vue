<template>
  <div class="calc">
    <a name="Calculator"></a>
    <div class="calc__title">
      <h3>Calculator</h3>
    </div>
    <div class="calc__content">
      <div class="content__item">
        <div class="inpSel">
          <input type="number" v-model="inpCur1" />
          <div class="select">
            <select name="" id="" v-model="selectCur1">
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
        <div class="chooseOper">
          <div
            class="operation"
            @click="selectOper = true"
            :style="{ backgroundColor: selectOper ? '#d6def3' : 'white' }"
          >
            <i class="fa-solid fa-plus"></i>
          </div>
          <div
            class="operation"
            @click="selectOper = false"
            :style="{ backgroundColor: !selectOper ? '#d6def3' : 'white' }"
          >
            <i class="fa-solid fa-minus"></i>
          </div>
        </div>
      </div>
      <div class="content__item">
        <div class="inpSel">
          <input type="number" v-model="inpCur2" />
          <div class="select">
            <select name="" id="" v-model="selectCur2">
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
        <div class="chooseOper">
          <div><i class="fa-solid fa-equals"></i></div>
        </div>
      </div>
      <div class="content__item">
        <div class="inpSel">
          <input type="number" disabled v-model="equal" />
          <div class="select">
            <select name="" id="" v-model="selectCur3">
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
    </div>
    <div class="calc__footer">
      {{ date }}
    </div>
  </div>
</template>

<script>
export default {
  props: {
    curr: {
      type: Array,
      required: true,
    },
    date: String,
  },
  data() {
    return {
      selectOper: true,
      selectCur1: "USD",
      selectCur2: "EUR",
      selectCur3: "JPY",
      inpCur1: 1,
      inpCur2: 1,
    };
  },
  computed: {
    equal() {
      let cur1 = this.curr.filter(
        (item) => item.Cur_Abbreviation === this.selectCur1
      )[0];
      let cur2 = this.curr.filter(
        (item) => item.Cur_Abbreviation === this.selectCur2
      )[0];
      let cur3 = this.curr.filter(
        (item) => item.Cur_Abbreviation === this.selectCur3
      )[0];
      if (cur1 && cur2 && cur3) {
        if (this.selectOper) {
          return (
            Math.round(
              (((this.inpCur1 * cur1.Cur_OfficialRate) / cur1.Cur_Scale +
                (this.inpCur2 * cur2.Cur_OfficialRate) / cur2.Cur_Scale) /
                (cur3.Cur_OfficialRate / cur3.Cur_Scale)) *
                100
            ) / 100
          );
        } else
          return (
            Math.round(
              (((this.inpCur1 * cur1.Cur_OfficialRate) / cur1.Cur_Scale -
                (this.inpCur2 * cur2.Cur_OfficialRate) / cur2.Cur_Scale) /
                (cur3.Cur_OfficialRate / cur3.Cur_Scale)) *
                100
            ) / 100
          );
      }
    },
  },
};
</script>

<style scoped>
.calc {
  display: flex;
  flex-direction: column;
  background-color: white;
  border-radius: 10px;
  padding: 20px;
  width: 46%;
  font-family: inherit;
  font-size: 12px;
  gap: 10px;
}

.calc__title,
.fa-solid {
  font-size: 1rem;
  color: #554a9d;
}

.calc__title {
  margin-bottom: 8px;
}

.operation:hover {
  cursor: pointer;
}

.calc__content {
  display: flex;
  flex-direction: column;
}

.chooseOper {
  display: flex;
  gap: 10px;
  margin-bottom: 10px;
}

.chooseOper div {
  padding: 3px 4px;
  border-radius: 5px;
  background-color: #d6def3;
  box-shadow: 0px 5px 10px 1px rgba(0, 0, 0, 0.2);
}

.calc__footer {
  display: flex;
  justify-content: flex-end;
}

input,
select {
  background-color: #f1edff;
  border: none;
  outline: none;
}

input {
  margin-bottom: 8px;
  width: 100%;
}

.select {
  display: flex;
  padding: 0 10px;
  background-color: #f1edff;
  border-radius: 10px;
  margin-bottom: 8px;
}

select:hover {
  cursor: pointer;
}

input {
  padding: 10px 20px;
  border-radius: 15px;
}

input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

.inpSel {
  display: flex;
  gap: 10px;
}
</style>