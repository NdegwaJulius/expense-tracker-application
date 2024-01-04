<template>
  <Header />
  <div class="container">
    <Balance  :total="+total"/>
    <IncomeExpenses :income="+income" :expenses="+expenses"/> 
    <TransactionLIst :transactions="transactions" @transactionDeleted="handleTransactionDeleted"/>
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted"/>
  </div>

</template>

<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpenses from './components/IncomeExpenses.vue';
import TransactionLIst from './components/TransactionLIst.vue';
import AddTransaction from './components/AddTransaction.vue';
import {useToast} from 'vue-toastification'

import {ref,computed} from 'vue'

const toast =useToast;
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
  //add transaction

  const  handleTransactionSubmitted=(transactionData)=>{
    transactions.value.push({
      id:generatedId(),
      text:transactionData.text,
      amount: transactionData.amount
    });
    toast.success('Transaction Added');
  };
  //generate unique id

  const generatedId=()=>{
    return Math.floor(Math.random() * 100000000000);
  };

  const handleTransactionDeleted = ()=>{
    transactions.value= transactions.value.filter((transaction)=> 
    transaction.id !== id
    );
    toast.success('Transaction Deleted');
  };
</script>