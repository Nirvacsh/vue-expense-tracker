<template>
  <Header />
  <div class="container">
    <Balance :total="+total" />
    <IncomeExpenses :expenses="+expenses" :income="+income" />
    <TransactionList
      :transactions="transactions"
      @transactionDeleted="handleTransactionDeleted"
    />
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
  </div>
</template>

<script setup>
import Header from './components/Header.vue'
import Balance from './components/Balance.vue'
import IncomeExpenses from './components/IncomeExpenses.vue'
import TransactionList from './components/TransactionList.vue'
import AddTransaction from './components/AddTransaction.vue'
import { useToast } from 'vue-toastification'

import { ref, computed, onMounted, watch } from 'vue'

onMounted(() => {
  const savedTransaction = JSON.parse(localStorage.getItem('transactions'))

  if (savedTransaction) {
    transactions.value = savedTransaction
  }
})

const toast = useToast()

const transactions = ref([])

// Get total
const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount
  }, 0)
})

// Get income
const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount
    }, 0)
})

// Get expenses
const expenses = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount
    }, 0)
})

// Add transaction
const handleTransactionSubmitted = ({ text, amount }) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: text,
    amount: amount,
  })

  toast.success('Transaction added')
}

// Delete transaction
const handleTransactionDeleted = (id) => {
  transactions.value = transactions.value.filter(
    (transaction) => transaction.id !== id
  )

  toast.success(`Transaction with id = ${id} deleted`)
}

// Generate unique ID
const generateUniqueId = () => {
  return Math.floor(Math.random() * 100000)
}

// Save to local storage
watch(
  transactions,
  () => {
    localStorage.setItem('transactions', JSON.stringify(transactions.value))
  },
  { deep: true }
)
</script>
