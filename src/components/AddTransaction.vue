<template>
    <h3>Add New Transaction</h3>
    <form id="form" @submit.prevent="onSubmit">
        <div class="form-control">
            <label for="text">Text</label>
            <input type="text" v-model="text" id="text" placeholder="Enter Text..."/>

        </div>
        <div class="form-control">
            <label for="amount">Amount <br/>(negative - expense, positive - income)</label>
            <input type="text" v-model="amount" id="amount" placeholder="Enter Amount"/>
        </div>
        <button value="primary-btn">Add Transaction</button>
    </form>
</template>
<script setup>
import { ref } from 'vue';
import { useToast } from 'vue-toastification';

const text = ref('');
const amount = ref(0);

const emit = defineEmits(['transactionSubmitted'])

const toast = useToast();

const onSubmit = () => {
  if(!text.value || !amount.value){
    toast.error('Both fields must be filled !!');
        return;
    
  }
const transactionData = {
    text: text.value,
    amount: parseFloat(amount.value)
};
console.log(amount.value+""+text.value)

emit('transactionSubmitted', transactionData)

  text.value = ""
  amount.value = ""
  };
</script>