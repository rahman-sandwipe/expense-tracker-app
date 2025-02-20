<script setup>
  import { ref, onMounted } from 'vue';
  import AddTransaction from './components/AddTransaction.vue';
  import TransactionList from './components/TransactionList.vue';

  const transactions = ref([]);

  // Load transactions from localStorage
  onMounted(() => {
    const savedTransactions = JSON.parse(localStorage.getItem('transactions')) || [];
    transactions.value = savedTransactions;
  });

  // Add a new transaction
  const addTransaction = (transaction) => {
    transactions.value.push(transaction);
    saveToLocalStorage();
  };

  // Delete a transaction
  const deleteTransaction = (id) => {
    transactions.value = transactions.value.filter(t => t.id !== id);
    saveToLocalStorage();
  };

  // Save transactions to localStorage
  const saveToLocalStorage = () => {
    localStorage.setItem('transactions', JSON.stringify(transactions.value));
  };
</script>


<template>
  <div class="container mt-5" style="width: 900px;">
    <h1>Expense Tracker</h1>
    <AddTransaction @add-transaction="addTransaction" />
    <TransactionList :transactions="transactions" @delete-transaction="deleteTransaction" />
  </div>
</template>
