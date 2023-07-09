<script setup>
    import { ref } from 'vue'
    import VueJsEssentials from './component/VueJsEssentials.vue'
    import Playground from './component/Playground.vue'

    const tabs = {
        VueJsEssentials,
        Playground,
    }
    const currentTab = ref('VueJsEssentials')
</script>

<template>
    <div class="main">
        <nav class="navbar">
            <div class="brand">
                <span class="logo">
                    <svg fill="none" viewBox="0 0 82 84" height="84" width="82" xmlns="http://www.w3.org/2000/svg" alt="Logo" class="h-[50px] w-[50px]"><title xmlns="http://www.w3.org/2000/svg">Home</title>
                        <path xmlns="http://www.w3.org/2000/svg" d="M0 36.2975L4.54024 42.2261L0 48.1495L6.0213 52.5485L3.31963 59.519L10.3317 62.0341L9.69332 69.4872L17.1242 69.9183L18.6053 77.2482L25.8574 75.5543L29.3302 82.1707L35.8112 78.4904L41 83.8543L46.1888 78.4904L52.6698 82.1707L56.1426 75.5543L63.3947 77.2482L64.8758 69.9183L72.3067 69.4872L71.6683 62.0341L78.6804 59.519L75.9787 52.5485L82 48.1495L77.4598 42.2261L82 36.2975L75.9787 31.8986L78.6804 24.9281L71.6683 22.4129L72.3067 14.9599L64.8758 14.5339L63.3947 7.20401L56.1426 8.89275L52.6698 2.28151L46.1888 5.9567L41 0.592773L35.8112 5.9567L29.3302 2.28151L25.8574 8.89275L18.6053 7.20401L17.1242 14.5339L9.69332 14.9599L10.3317 22.4129L3.31963 24.9281L6.0213 31.8986L0 36.2975Z" fill="white"></path>
                        <path xmlns="http://www.w3.org/2000/svg" d="M20.9512 26.7803L40.691 60.3675L60.4309 26.7803L52.535 26.7803L40.691 46.9326L28.7484 26.7803L20.9512 26.7803Z" fill="#3DD491"></path>
                        <path xmlns="http://www.w3.org/2000/svg" d="M28.8452 26.7803L40.7383 47.196L52.5331 26.7803L45.2596 26.7803L40.7383 34.6325L36.1187 26.7803L28.8452 26.7803Z" fill="#2C3D4F"></path>
                    </svg>
                </span>
                <span class="name">
                    Vue.js Certification Preparation
                </span>
            </div>
            <div class="tabs">
                <span
                    v-for="(_, tab) in tabs" 
                    :key="tab"
                    :class="['tab-button',{ active: currentTab === tab }]"
                    @click="currentTab=tab"
                >
                    {{ tab }}
                </span>
            </div>
        </nav>
        <div class="container">
            <div class="component-title">{{ currentTab }} {{ currentTab === 'Playground' ? '' : 'Coding Challenge' }}</div>
            <component :is="tabs[currentTab]" ></component>
        </div>
    </div>
</template>

<style>
    .main{
        /* @apply  */
    }
    .main .navbar{
        @apply p-3 flex shadow text-white sticky top-0 z-50;
        background: #191919;
    }
    .main .navbar .brand{
        @apply flex gap-2 items-center;
    }
    .main .navbar .brand .logo{
        @apply shadow;
    }
    .main .navbar .brand .name{
        @apply text-white text-2xl;
    }
    .main .navbar .tabs{
        @apply ms-auto flex items-center gap-2;
    }
    .main .navbar .tabs .tab-button{
        @apply flex py-1 hover:border-b-2 cursor-pointer;
    }
    .main .navbar .tabs .tab-button.active{
        @apply border-b-2;
    }
    .main .container{
        @apply mx-auto py-10;
    }
    .main .container .component-title{
        @apply mb-3;
    }
</style>
