<script setup>
import { ref } from 'vue'
import { useToast } from 'vue-toastification'

const toast = useToast()

const text = ref(null)
const amount = ref(null)

const emit = defineEmits(['transactionSubmitted'])

const onSubmit = () => {
  if (!text.value || !amount.value) {
    toast.error('Both form fields must be filled')
    return
  }
  const transactionData = {
    text: text.value,
    amount: parseFloat(amount.value)
  }

  emit('transactionSubmitted', transactionData)
  text.value = ''
  amount.value = ''
}
</script>

<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Label</label>
      <input type="text" id="text" v-model.trim="text" placeholder="Enter label..." />
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount <br />
        (negative - expense, positive - income)</label
      >
      <input type="text" id="amount" v-model.trim="amount" placeholder="Enter amount..." />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>
