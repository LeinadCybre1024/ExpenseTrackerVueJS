<template>
<Header/>
<div class="container">
  <Balance :total="total"/>
  <IncomeExpense :incomeTotal="incomeTotal" :expenseTotal="expenseTotal"/>
  <TransactionList :transactions="transactions" @transationdeleted="handleTransactionDeleted"/>
  <AddTransaction @transactionSubmitted="handleTransactionSubmitted"/>
</div>
</template>

<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpense from './components/IncomeExpense.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';
import { useToast } from 'vue-toastification';
import { ref, computed, onMounted } from 'vue';

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'))

  if(savedTransactions){
    transactions.value = savedTransactions
  }
});

const toast = useToast()

//List trnsactions
const transactions = ref([
    { id: 1, text: 'Flower', amount: -19.99 },
    { id: 2, text: 'Salary', amount: 299.97 },
    { id: 3, text: 'Book', amount: -10 },
    { id: 4, text: 'Camera', amount: 150 },
  ]);

  //Get Total Transaction Amount
  const total = computed(()=> {
    return transactions.value.reduce((acc, transaction) => {
      return acc + transaction.amount
    }, 0);
  });

  //Get Income
  const incomeTotal = computed(()=> {
    return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0)
    .toFixed(2);
  });

  //Get Expenses
  const expenseTotal = computed(()=>{
    return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0)
    .toFixed(2)
  });

  //HandleSave Transaction
  const  handleTransactionSubmitted = (transactionData) => {
    transactions.value.push({
      id: generateUniqueId(),
      text: transactionData.text,
      amount: transactionData.amount
    });

    saveToLocalStorage()

    toast.success("Transaction Added")
  };
  const generateUniqueId = () => {
    return Math.floor(Math.random() * 10000)
  };
  const saveToLocalStorage = () => {
    localStorage.setItem('transactions', JSON.stringify(transactions.value))
  }

  //Handle delete
  const handleTransactionDeleted = (id) => {
    transactions.value = transactions.value.filter(
      (transaction) => transaction.id !== id
    );
    saveToLocalStorage();
    toast.success('Transaction Removed');
  };

</script>
