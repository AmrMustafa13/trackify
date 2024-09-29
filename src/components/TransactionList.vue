<template>
  <h3>History</h3>
  <p v-if="transactions.length === 0">No transactions yet</p>
  <ul id="list" class="list" v-else>
    <li
      v-for="transaction in transactions"
      :key="transaction.id"
      :class="transaction.amount < 0 ? 'minus' : 'plus'"
    >
      {{ transaction.text }}
      <span>${{ transaction.amount }}</span
      ><button @click="deleteTransaction(transaction.id)" class="delete-btn">
        x
      </button>
    </li>
  </ul>
</template>

<script setup>
const { transactions } = defineProps({
  transactions: {
    type: Array,
    required: true,
  },
});

const emit = defineEmits(["deleteTransaction"]);

const deleteTransaction = (id) => {
  emit("deleteTransaction", id);
};
</script>
