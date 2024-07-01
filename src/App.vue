<template>
  <Header />
  <div class="container">
    <Balance :total="+total" />
    <incomeExpences :income="+income" :expenses="+expenses" />
    <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted"/>
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted"/>

  </div>
</template>

<script setup>
  import Header from './components/Header.vue';
  import Balance from './components/Balance.vue';
  import incomeExpences from './components/incomeExpences.vue';
  import TransactionList from './components/TransactionList.vue';
  import AddTransaction from './components/AddTransaction.vue';
  import {useToast} from 'vue-toastification';
  import { ref, computed, onMounted} from 'vue';

    const toast = useToast();
    const transactions = ref([

    ]);
    onMounted(() => {
      const savedTransactions = JSON.parse(localStorage.getItem('transactions'));
      if(savedTransactions) {
        transaction.value = savedTransactions;
      }
    })

    // Get total
    const total = computed(() => {
      return transactions.value.reduce((acc, transaction) => {
        return acc +transaction.amount;
      }, 0);
    });

// Get income
  const income = computed(() => {
        return transactions.value
        .filter((transaction) => transaction.amount > 0)
        .reduce((acc, transaction) => {
          return acc +transaction.amount;
        }, 0).toFixed(2);
      });
//Get expenses
const expenses = computed(() => {
        return transactions.value
        .filter((transaction) => transaction.amount < 0)
        .reduce((acc, transaction) => {
          return acc +transaction.amount;
        }, 0).toFixed(2);
      });

  //Add transaction

const handleTransactionSubmitted = (transactionData) => {
  transactions.value.push ({
    id: generateUniqueId(),
    text: transactionData.text,
    amount: transactionData.amount,

  });

  savedTransactionsToLocalStorage();

  toast.success('Transaction added');
};

//Generate unique Id
const generateUniqueId = () => {
  return Math.floor(Math.random() * 1000000);
}

// Delete transaction 
const handleTransactionDeleted = (id) => {
  transactions.value = transactions.value.filter((transaction) => 
  transaction.id !== id);

  savedTransactionsToLocalStorage();

  toast.success('Transaction deleted');
};

//Save to Localstorage

const savedTransactionsToLocalStorage = () => {
  localStorage.setItem('transaction', JSON.stringify(transaction.value))
}
</script>