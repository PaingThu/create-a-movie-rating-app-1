<script setup>
    import { ref } from "vue"
    import AlertBox from "./AlertBox.vue"
    import FontSizeController from '/src/component/FontSizeController.vue'
    
    const postFontSize = ref(1)
</script>
<template>
    <div class="min-h-screen flex flex-col gap-3">
        <div class="section flex flex-col gap-3">
            <h3 class="underline underline-offset-4 pb-3">
                Listening to events
            </h3>
            <FontSizeController 
                @increase-by="(n) => postFontSize += n"
            />
            <div :style="{ fontSize: postFontSize + 'em'}">
                <span>My Journey with vue.</span>
            </div>
        </div>
        <hr class="hr">
        <div class="section content-distribution-with-slots">
            <h3 class="underline underline-offset-4 pb-3">
                Content Distribution with Slots
            </h3>
            <AlertBox>Something bad happened.</AlertBox>
        </div>
        <hr class="hr">
    </div>
</template>
<style>
    .hr{
        @apply border-gray-300 my-5;
        border-bottom-width: 0.25px;
    }
</style>