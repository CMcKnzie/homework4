<script setup>

import AddTransactions from './components/AddTransactions.vue'
import Balance from './components/Balance.vue';
import Header from './components/Header.vue';
import IncomeExpenses from './components/IncomeExpenses.vue';
import TransactionHistory from './components/TransactionHistory.vue'

import {ref, computed, onMounted} from 'vue'

const transactions = ref([])

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
  id:generateID(),
  
  })
  saveToLocalStorage()
}

const generateID =() => {
  return Math.floor(Math.random() * 10000000)
}

const handleDelete =(id) => {
  transactions.value = transactions.value.filter((x)=> x.id !== id)
  saveToLocalStorage()

}

const saveToLocalStorage=() =>{
  localStorage.setItem('transactions', JSON.stringify(transactions.value))
}

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'))
  if(savedTransactions){
    transactions.value=savedTransactions
  }
})

</script>

<template>
  <Header></Header>

  <div class ="container">
      <Balance :total="sum"></Balance>
      <AddTransactions @transactionSubmitted="handleTransaction"></AddTransactions>
      <IncomeExpenses :income="moneyIn" :expense="moneyOut"></IncomeExpenses>
      <TransactionHistory :transactions="transactions" @transactionDeleted="handleDelete"></TransactionHistory>

    </div>

</template>