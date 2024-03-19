<template>
    <h3>History</h3>
    <ul id="list" class="list">
        <i v-if="!transactions || transactions.length==0">No transactions available</i>
        <li v-for="transaction in transactions" v-bind:key="transaction.id" :class="transaction.amount<0 ? 'minus' : 'plus'">
            {{transaction.text}} <span>{{transaction.amount<0?'-':''}}£{{Number(Math.abs(transaction.amount)).toLocaleString(undefined, { minimumFractionDigits: 2 })}}</span>
            <button class="delete-btn" @click="deleteTransaction(transaction.id)"/>
        </li>
    </ul>
</template>

<script setup>
    import { defineProps } from 'vue';

    const props = defineProps({transactions: {type:Array, required: true}});

    const emit = defineEmits(['transactionDeleted']);

    const deleteTransaction = (id) => emit('transactionDeleted', id);
</script>