<script setup lang="ts">
import Header from './components/Header.vue'
import Balance from './components/Balance.vue'
import IncomeExpense from './components/IncomeExpense.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';
import { ref, computed, onMounted, watch } from 'vue';
import { useToast } from 'vue-toastification';

interface Transaction {
  id: number;
  text: string;
  amount: number;
}

const transactions = ref<Transaction[]>([]);

onMounted(() => {
  const savedTransactions = localStorage.getItem('transactions');
  if (savedTransactions) {
    transactions.value = JSON.parse(savedTransactions);
  }
});

// Calculate total
const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => (acc += transaction.amount), 0).toFixed(2);
});

// Calculate income
const income = computed(() => {
  return transactions.value
    .filter(transaction => transaction.amount > 0)
    .reduce((acc, transaction) => (acc += transaction.amount), 0)
    .toFixed(2);
});

// Calculate expense
const expense = computed(() => {
  return transactions.value
    .filter(transaction => transaction.amount < 0)
    .reduce((acc, transaction) => (acc += transaction.amount), 0) * -1
    .toFixed(2);
});

const toast = useToast();

// Save transactions to local storage
const saveTransactionsToLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value));
}

// transactionSubmitted event handler
const handleTransactionSubmitted = (transaction: Transaction) => {
  transactions.value.push({
    id: transaction.id,
    text: transaction.text,
    amount: transaction.amount
  });

  saveTransactionsToLocalStorage();

  // if transaction added to transactions array, show success toast
  toast.success('Transaction added successfully');
};

const handleTransactionDeleted = (id: number) => {
  transactions.value = transactions.value.filter(transaction => transaction.id !== id);
  saveTransactionsToLocalStorage();
  toast.error('Transaction deleted successfully');
};

// Watch transactions array and save it to local storage
watch(transactions, saveTransactionsToLocalStorage, { deep: true });

</script>

<template>
  <Header />

  <div class="container">
    <Balance :total="+total" />
    <IncomeExpense :income="+income" :expense="+expense" />
    <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted"/>
    <AddTransaction @transactionSubmitted = "handleTransactionSubmitted"/>
  </div>

</template>

<style scoped></style>