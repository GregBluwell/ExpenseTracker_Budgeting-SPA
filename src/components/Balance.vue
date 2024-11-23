<script setup>
import { defineProps } from 'vue';


const props = defineProps({
    total: {
        type: Number,
        required: true
    }
});


</script>
<template>


    <h4>Your Balance</h4>

    <h1 id="balance" :style="total > 0 ? 'color: #2ecc71' : 'color: #c0392b'"> <!-- GET THE RIGHT COLORS -->


        {{ total }} €
    </h1>
</template>
