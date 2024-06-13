<template>
  <h3>Add new transaction</h3>
  <form @submit.prevent="onSubmit" id="form">
    <div class="form-control">
      <label for="text">Text</label>
      <input type="text" placeholder="Enter text..." id="text" v-model="text" />
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount <br />
        (negative - expense, positive - income)
      </label>
      <input
        type="number"
        id="amount"
        step="0.01"
        placeholder="Enter amount..."
        v-model="amount"
      />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>

<script setup>
import { ref } from 'vue'
import { useToast } from 'vue-toastification'

const text = ref('')
const amount = ref('')
const toast = useToast()

const emit = defineEmits(['transactionSubmitted'])

const onSubmit = () => {
  if (!text.value || !amount.value) {
    toast.error('Both fields are required')
    return
  }

  const transactionData = {
    text: text.value,
    amount: parseFloat(amount.value),
  }

  emit('transactionSubmitted', transactionData)

  text.value = ''
  amount.value = ''
}
</script>
