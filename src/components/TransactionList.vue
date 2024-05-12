<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li
      v-for="transaction in transactions"
      :key="transaction.id"
      :class="transaction.amount < 0 ? 'minus' : 'plus'"
    >
      <span v-if="!transaction.editingText">{{ transaction.text }}</span>
      <input
        v-else
        v-model="transaction.text"
        @keyup.enter="saveEdit(transaction)"
        @keyup.esc="cancelEdit(transaction)"
      />
      <span v-if="!transaction.editingAmount">${{ transaction.amount }}</span>
      <input
        v-else
        type="number"
        v-model.number="transaction.amount"
        @keyup.enter="saveEdit(transaction)"
        @keyup.esc="cancelEdit(transaction)"
      />
      <button class="edit-btn" @click="editTransaction(transaction)">Edit</button>
      <button
        v-if="transaction.editingText || transaction.editingAmount"
        class="cancel-btn"
        @click="cancelEdit(transaction)"
      >
        Cancel
      </button>
      <button
        v-if="transaction.editingText && transaction.editingAmount"
        class="update-btn"
        @click="saveEdit(transaction)"
      >
        Update
      </button>
      <button
        v-else-if="transaction.editingText && !transaction.editingAmount"
        class="update-btn"
        @click="saveEdit(transaction)"
      >
        Save
      </button>
      <button class="delete-btn" @click="deleteTransaction(transaction.id)">Delete</button>
    </li>
  </ul>
</template>

<script setup>
import { defineProps, defineEmits } from 'vue'

const emit = defineEmits(['transactionDeleted', 'transactionUpdated'])
defineProps({
  transactions: {
    type: Array,
    required: true
  }
})

const deleteTransaction = (id) => {
  emit('transactionDeleted', id)
}

const editTransaction = (transaction) => {
  transaction.editingText = true
  transaction.editingAmount = true
  transaction.originalText = transaction.text
  transaction.originalAmount = transaction.amount
}

const saveEdit = (transaction) => {
  if (transaction.text.trim() !== '' && !isNaN(transaction.amount)) {
    emit('transactionUpdated', {
      id: transaction.id,
      text: transaction.text,
      amount: transaction.amount
    })
  } else {
    transaction.text = transaction.originalText
    transaction.amount = transaction.originalAmount
  }
  cancelEdit(transaction)
}

const cancelEdit = (transaction) => {
  transaction.editingText = false
  transaction.editingAmount = false
}
</script>
