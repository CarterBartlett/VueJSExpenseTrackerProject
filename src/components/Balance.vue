<template>
    <h4>Your Balance</h4>
    <h1 id="balance">{{total<0?"-":""}}£{{Number(Math.abs(total)).toLocaleString(undefined, { minimumFractionDigits: 2 })}}</h1>
</template>

<script setup>
    import { defineProps } from 'vue';

    const props = defineProps({total: {type:Number, required: true}});
</script>