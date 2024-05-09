<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li
      v-for="transaction in transactions"
      :key="transaction.id"
      :class="transaction.amount < 0 ? 'minus' : 'plus'"
    >
      {{ transaction.text }} <span>${{ transaction.amount }}</span
      ><button class="delete-btn">x</button>
    </li>
  </ul>
</template>

<script setup>
const transactions = [
  { id: 1, text: 'Grocery shopping', amount: -50.25 },
  { id: 2, text: 'Venmo', amount: 30.0 },
  { id: 3, text: 'Restaurant dinner', amount: -75.8 },
  { id: 4, text: 'Online sale', amount: 120.5 }
]
</script>
