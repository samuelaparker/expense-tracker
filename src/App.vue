<template>
  <HeaderItem />
  <div class="container">
    <BalanceItem />
    <IncomeExpenses />
    <TransactionList />
    <AddTransaction />
  </div>
</template>

<script>
import HeaderItem from './components/HeaderItem.vue'
import BalanceItem from './components/BalanceItem.vue'
import IncomeExpenses from './components/IncomeExpenses.vue'
import TransactionList from './components/TransactionList.vue'
import AddTransaction from './components/AddTransaction.vue'
export default {
  components: {
    HeaderItem,
    BalanceItem,
    IncomeExpenses,
    TransactionList,
    AddTransaction
  }
}
</script>
