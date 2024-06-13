<template>
  <div class="inc-exp-container">
    <div>
      <h4>Income</h4>
      <p class="money plus" id="money-plus">{{ income }}$</p>
    </div>
    <div>
      <h4>Expense</h4>
      <p class="money minus" id="money-minus">{{ expenses }}$</p>
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  income: {
    type: Number,
    required: true,
  },
  expenses: {
    type: Number,
    required: true,
  },
})
</script>
