<template>
    <Header />
    <div class="container">
        <Balance :total="total" />
        <IncomeExpenses :income="income" :expenses="expenses"/>
        <TransactionList :transactions="transactions" @transaction-deleted="handleTransactionDeleted" />
        <AddTransaction @transaction-submitted="handleTransactionSubmitted" />
    </div>
</template>

<script setup>
    import Header from './components/Header.vue';
    import Balance from './components/Balance.vue';
    import IncomeExpenses from './components/IncomeExpenses.vue';
    import TransactionList from './components/TransactionList.vue';
    import AddTransaction from './components/AddTransaction.vue';

    import {useToast} from 'vue-toastification';

    const toast = useToast();

    import {ref, computed, onMounted} from 'vue';

    const transactions = ref([]);

    onMounted(()=>{
        const savedTransactions = JSON.parse(localStorage.getItem('transactions'));
        if (savedTransactions) transactions.value=savedTransactions;
    })

    const total = computed(()=>{
        return transactions.value.reduce((acc,t)=>acc+Number.parseFloat(t.amount), 0);
    });
    const income = computed(()=>{
        return transactions.value.filter(t=>t.amount>0).reduce((acc,t)=>acc+Number.parseFloat(t.amount), 0);
    });
    const expenses = computed(()=>{
        return Math.abs(transactions.value.filter(t=>t.amount<0).reduce((acc,t)=>acc+Number.parseFloat(t.amount), 0));
    });

    const handleTransactionSubmitted = (data) => {
        transactions.value.push({
            id: generateUniqueId(),
            text: data.text,
            amount: data.amount
        });
        saveTransactionsToLocalStorage();
        toast.success('Transaction submitted');
    };

    const handleTransactionDeleted = (id) => {
        transactions.value = transactions.value.filter(t=>t.id!==id);
        saveTransactionsToLocalStorage();
        toast.success('Transaction deleted')
    }

    const generateUniqueId = () => Math.floor(Math.random() * 1000000);

    // Local storage save
    const saveTransactionsToLocalStorage = () => localStorage.setItem('transactions', JSON.stringify(transactions.value));
</script>