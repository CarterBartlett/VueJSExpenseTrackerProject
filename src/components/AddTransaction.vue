<template>
    <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input type="text" id="text" placeholder="Enter text..." v-model="text" />
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount <br />
        (negative - expense, positive - income)</label
      >
      <input
        type="text"
        id="amount"
        placeholder="Enter amount..."
        v-model="amount"
      />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>

<script setup>
import { ref } from 'vue';
import {useToast} from 'vue-toastification';

const toast = useToast();

const text = ref('');
const amount = ref('');

const emit = defineEmits(['transactionSubmitted']);

  const onSubmit = () => {
    if (!text.value || !amount.value)
      return toast.error("Both fields must be filled")

    if (isNaN(amount.value))
      return toast.error("Amount must be a valid number")
    if (amount.value==0)
      return toast.error("Amount cannot be 0")
    if (amount.value==1e10000 || amount.value==-1e10000)
      return toast.error("Amount cannot be infinity")

    emit('transactionSubmitted', {
      text: text.value,
      amount: parseFloat(amount.value)
    });

    text.value='';
    amount.value='';
  }
</script>