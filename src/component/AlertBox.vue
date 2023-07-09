<template>
    <div class="alert-box">
        <strong>Error!</strong>
        <br />
        <slot />
    </div>
</template>
<style scoped>
    .alert-box{
        @apply border border-red-400 p-5 rounded bg-gray-300 ;
    }
    strong{
        color: red;
    }
</style>