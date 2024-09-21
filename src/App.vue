<script setup>

import AddTransactions from './components/AddTransactions.vue'
import Balance from './components/Balance.vue';
import Header from './components/Header.vue';
import IncomeExpenses from './components/IncomeExpenses.vue';

import {ref, computed} from 'vue'

const transactions = ref([

])

const sum = computed(() => {
  return transactions.value.reduce((acc, x) => {
      return acc+x.amount
  },0)
})

const moneyIn = computed(() => {
  return transactions.value
  .filter((x)=>x.amount>0)
  .reduce((acc, x) => {
      return acc+x.amount
  },0)
})

const moneyOut = computed(() => {
  return transactions.value
  .filter((x)=>x.amount<0)
  .reduce((acc, x) => {
      return acc+x.amount
  },0)
})

const handleTransaction = (transactionData) => {
  transactions.value.push ({
  text: transactionData.text,
  amount: transactionData.amount, 
  })
}

</script>

<template>
  <Header></Header>

  <div class ="container">
      <Balance :total="sum"></Balance>
      <AddTransactions @transactionSubmitted="handleTransaction"></AddTransactions>
      <IncomeExpenses :income="moneyIn" :expense="moneyOut"></IncomeExpenses>

    </div>

</template>