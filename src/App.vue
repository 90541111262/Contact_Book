<template>
  <div class="container">
    <ContactList :transactions="transactions" @delete="deleteTransaction" @edit="selectTransactionForEdit" />
    <ContactAdd @add="addTransaction" @update="updateTransaction" :transaction="selectedTransaction" />
  </div>
</template>
<script setup>
import ContactList from './components/ContactList.vue';
import ContactAdd from './components/ContactAdd.vue';
import { ref, onMounted, reactive } from 'vue';
import { useToast } from 'vue-toastification';
// Get toast interface
const toast = useToast();

const transactions = ref([]);
const selectedTransaction = ref(null);

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'));
  if (savedTransactions) {
    transactions.value = savedTransactions;
  }
});

// Save transactions to local storage
const saveTransactionsToLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value));
};

const addTransaction = (transactionData) => {
  transactions.value.push({
    id: generateUniqueId(),
    name: transactionData.name,
    fone: transactionData.fone,
    address: transactionData.address,
  });
  saveTransactionsToLocalStorage();
  toast.success('Contact added.');
};

const selectTransactionForEdit = (transaction) => {
  selectedTransaction.value = { ...transaction };
}

const updateTransaction = (updatedTransaction) => {
  const index = transactions.value.findIndex(transaction => transaction.id === updatedTransaction.id);
  if (index !== -1) {
    transactions.value[index] = { ...updatedTransaction };
  }
  saveTransactionsToLocalStorage();
  toast.success('Contact Updated.');
  selectedTransaction.value = null;
};

const deleteTransaction = (id) => {
  transactions.value = transactions.value.filter(
    (transaction) => transaction.id !== id
  );
  saveTransactionsToLocalStorage();
  toast.success('Contact deleted.');
};

const generateUniqueId = () => {
  return Math.floor(Math.random() * 1000000);
};

</script>