<template>
  <div class="inc-exp-container">
    <div>
        <h4>Income</h4>
        <p id="money-plus" class="money plus">${{ income }}</p>
    </div>
    <div>
        <h4>Expence</h4>
        <p id="money-plus" class="money minus">${{ expenses }}</p>
    </div>
  </div>  
</template>

<script setup>
    import {defineProps} from 'vue';

    const props = defineProps({
        income: {
            type: Number,
            required: true,
        },
        expenses: {
            type: Number,
            required: true,
        }

    })
</script>