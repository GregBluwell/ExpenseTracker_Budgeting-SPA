
<script setup>
import { ref } from 'vue';
import {useToast} from 'vue-toastification';
import { defineEmits } from 'vue';

// onsubmit function, emit addTransaction event

const transactionID = ref(1);
const text = ref('');
const amount = ref(null);

const emit = defineEmits(['transactionSubmitted']);

const toast = useToast();

const onSubmit = () => {
    if (isNaN(amount.value)) {
        toast.error('Please add a valid amount');
        return;
    }
    else if (!text.value || !amount.value) {
        toast.error('Please add a text and amount');
        return;
    }


    const transactionData = {
        id: transactionID.value++,
        text: text.value, 
        amount: parseFloat(amount.value)
    };

    // Emit addTransaction event
    emit('transactionSubmitted', transactionData);


    text.value = '';
    amount.value = '';
};




</script>


<template>
    <h3>Add new transaction</h3>


    <form id="form" @submit.prevent="onSubmit">


        <div class="form-control">

            <label for="text">Text</label>

            <input type="text" id="text" v-model="text" placeholder="Enter text..." />

        </div>

        <div class="form-control">
            <label for="amount">Amount</label>

            <input
            type="text" id="amount"
            v-model="amount" 
            placeholder="Enter amount..." /> 
        </div>

        <button class="btn">Add transaction</button>
    </form>
</template>