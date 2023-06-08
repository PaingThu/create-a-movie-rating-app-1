<script setup>
import { reactive } from "vue"
import { items } from "./movies.json"
/*
 This is an Icon that you can use to represent the stars if you like
 otherwise you could just use a simple ⭐️ emoji, or * character.
*/
// import { StarIcon } from "@heroicons/vue/24/solid";
console.log("Movies Items ", items)
const movies = reactive(items)
</script>

<template>
    <!-- This is where your template goes	-->
    <div class="bg-slate-950 h-screen">
        <div class="grid grid-cols-3 p-20 gap-3">
            <template v-for="(movie, index) in movies" :key="index">
                <div class="flex flex-col gap-2 rounded bg-white">
                    <div class="" style="height: 475px;" >
                        <img :src="movie.image" alt="">
                    </div> 
                    <div class="flex flex-col gap-1 bg-white p-3 h-full">
                        <h3 class="text-xl">{{ movie.name }}</h3>
                        <div class="flex gap-2">
                            <span class=" bg-blue-600 rounded-full px-3 text-white" v-for="(cat,catIndex) in movie.genres" :key="catIndex">{{ cat }}</span>
                        </div>
                        <p class="mt-2">{{ movie.description }}</p>
                        <div class="flex h-full gap-2 items-end">
                            <small>Rating: ({{ movie.rating }}/5)</small>
                            <div class="flex gap-1">
                                <small v-for="(star) in movie.rating" :key="star" class="text-orange-300">&#9733;</small>
                            </div>
                        </div>
                    </div>
                </div>
            </template>
        </div>
    </div>
</template>
