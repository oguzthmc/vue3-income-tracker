<template>
  <Header :totalIncome="state.totalIncome" />
  <Form @add-income="AddIncome" />
  <IncomeList :state="state" />
</template>

<script>
import { reactive, computed } from "vue";

import Header from './components/Header';
import Form from './components/Form';
import IncomeList from './components/IncomeList';

export default {
  name: "App",
  setup() {
    const state = reactive({
      income: [],
      totalIncome: computed(() => {
        let sum = 0;
        if (state.income.length > 0) {
          for (let i = 0; i < state.income.length; i++) {
            sum += state.income[i].value;
          }
        }
        return sum;
      }),
      sortedIncome: computed(() => {
        let inline = [];

        inline = state.income.sort(function(a, b) {
          return b.date - a.date;
        })

        return inline;
      })
    });

    function AddIncome(data) {
      let dt = data.date.split("-");
      let newDt = new Date(dt[0], dt[1], dt[2]);

      state.income = [...state.income, {
        id: Date.now(),
        desc: data.desc,
        value: parseInt(data.value),
        date: newDt.getTime(),
      }];

      console.log(state.income);
    }

    return {
      Header,
      Form,
      state,
      AddIncome,
      IncomeList
    };
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Fira Sans", sans-serif;
}

body {
  background: #eee;
}
</style>
