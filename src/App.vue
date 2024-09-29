<template>
  <Header />
  <div class="container">
    <Balance :total="+total" />
    <IncomeExpenses :income="+income" :expense="+expense" />
    <TransactionList
      :transactions="transactions"
      @deleteTransaction="deleteTransaction"
    />
    <AddTransaction @addTransaction="handleAddTransaction" />
  </div>
</template>

<script setup>
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";

import { ref, computed, onMounted } from "vue";

import { useToast } from "vue-toastification";

const transactions = ref([]);

onMounted(() => {
  transactions.value = JSON.parse(localStorage.getItem("transactions")) || [];
});

const toast = useToast();

const total = computed(() => {
  return transactions.value.reduce((acc, item) => (acc += item.amount), 0);
});

const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, item) => (acc += item.amount), 0);
});

const expense = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, item) => (acc += item.amount), 0);
});

const handleAddTransaction = (newTransaction) => {
  transactions.value.push(newTransaction);
  updateLocalStorage();
  toast.success("Transaction added successfully!");
};

const deleteTransaction = (id) => {
  transactions.value = transactions.value.filter(
    (transaction) => transaction.id !== id
  );
  updateLocalStorage();
  toast.success("Transaction deleted successfully!");
};

const updateLocalStorage = () => {
  localStorage.setItem("transactions", JSON.stringify(transactions.value));
};
</script>
