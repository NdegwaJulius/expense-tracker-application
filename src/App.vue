<template>
  <Header />
  <div class="container">
    <Balance  :total="total"/>
    <IncomeExpenses :income="income" :expenses="expenses"/> 
    <TransactionLIst :transactions="transactions" />
    <AddTransaction />
  </div>

</template>

<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpenses from './components/IncomeExpenses.vue';
import TransactionLIst from './components/TransactionLIst.vue';
import AddTransaction from './components/AddTransaction.vue';


import {ref,computed} from 'vue'


const transactions =ref([
  { id: 1, text: 'Flower', amount: -2900 },
  { id: 2, text: 'Salary', amount: 3080 },
  { id: 3, text: 'Book', amount: -10 },
  { id: 4, text: 'Camera', amount: 1580 }
   ]);
  // Get tottal
  const total = computed(() =>{
    return transactions.value.reduce((acc,transaction ) =>{
      return acc + transaction.amount;
    },  0);
  });

  //Get income
  const income = computed(() =>{
    return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc,transaction ) =>{
      return acc + transaction.amount;
    },  0).toFixed(2);
  });

  // Get expenses
  const expenses = computed(() =>{
    return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc,transaction ) =>{
      return acc + transaction.amount;
    },  0).toFixed(2);
  });
</script>