<script setup>
    const emit = defineEmits(['increase-by'])
</script>
<template>
    <!-- <button @click="$emit('increase-by', 0.1)">+</button> -->
    <div class="flex gap-1">
        <button class="button" @click="emit('increase-by', 0.1)">+</button>
        <button class="button" @click="emit('increase-by', -0.1)">-</button>
    </div>
</template>

<style>
    .button{
        @apply w-8 h-8 flex items-center justify-center;
    }
</style>