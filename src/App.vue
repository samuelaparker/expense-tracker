<template>
  <HeaderItem />
  <div class="container">
    <BalanceItem :total="total" />
    <IncomeExpenses :income="income" :expenses="expenses" />
    <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted" />
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
  </div>
</template>

<script setup>
import HeaderItem from './components/HeaderItem.vue'
import BalanceItem from './components/BalanceItem.vue'
import IncomeExpenses from './components/IncomeExpenses.vue'
import TransactionList from './components/TransactionList.vue'
import AddTransaction from './components/AddTransaction.vue'

import { ref, computed, onMounted } from 'vue'
import { useToast } from 'vue-toastification'

const toast = useToast()

const transactions = ref([])

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'))
  if (savedTransactions) {
    transactions.value = savedTransactions
  }
})

//Get total
const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    let sum = Math.round((acc + transaction.amount) * 100) / 100
    return sum
  }, 0)
})

//Get income
const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => {
      let sum = Math.round((acc + transaction.amount) * 100) / 100
      return sum
    }, 0)
})

//Get expenses
const expenses = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
      let sum = Math.round((acc + transaction.amount) * 100) / 100
      return sum
    }, 0)
})

//Add transaction
const handleTransactionSubmitted = (transactionData) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: transactionData.text,
    amount: transactionData.amount
  })

  saveTransactionsToLocalStorage()

  toast.success('Transaction added')
}

//Generate unique ID
const generateUniqueId = () => {
  return Math.floor(Math.random() * 1000000)
}

//Delete transaction
const handleTransactionDeleted = (id) => {
  transactions.value = transactions.value.filter((transaction) => {
    return transaction.id !== id
  })
  saveTransactionsToLocalStorage()

  toast.success('Transaction deleted')
}

//Save to localStorage
const saveTransactionsToLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value))
}
</script>
