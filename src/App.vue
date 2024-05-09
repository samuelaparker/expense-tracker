<template>
  <HeaderItem />
  <div class="container">
    <BalanceItem :total="total" />
    <IncomeExpenses :income="income" :expenses="expenses" />
    <TransactionList :transactions="transactions" />
    <AddTransaction />
  </div>
</template>

<script setup>
import HeaderItem from './components/HeaderItem.vue'
import BalanceItem from './components/BalanceItem.vue'
import IncomeExpenses from './components/IncomeExpenses.vue'
import TransactionList from './components/TransactionList.vue'
import AddTransaction from './components/AddTransaction.vue'

import { ref, computed } from 'vue'

const transactions = ref([
  { id: 1, text: 'Grocery shopping', amount: -50.25 },
  { id: 2, text: 'Venmo', amount: 30 },
  { id: 3, text: 'Restaurant dinner', amount: -75.83 },
  { id: 4, text: 'Online sale', amount: 120.53 }
])

//Get total
const total = computed(() => {
  return transactions.value
    .reduce((acc, transaction) => {
      return acc + transaction.amount
    }, 0)
    .toFixed(2)
})

//Get income
const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount
    }, 0)
    .toFixed(2)
})

//Get expenses
const expenses = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount
    }, 0)
    .toFixed(2)
})
</script>
