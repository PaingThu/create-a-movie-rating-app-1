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
const ratingMovie = (star,index) => {
    if(movies[index].rating != star){
            movies[index].rating = star
    }
}
const configureClass = (star, movie) => {
    const color = star <= movie.rating ? 'text-orange-300' : 'text-gray-300'
    const pointer = star == movie.rating ? 'cursor-not-allowed' : 'cursor-pointer'
    return `${color} ${pointer}`
}
</script>

<template>
    <!-- This is where your template goes	-->
    <div class="bg-slate-950">
        <div class="grid grid-cols-3 p-20 gap-3">
            <template v-for="(movie, index) in movies" :key="index">
                <div class="flex flex-col gap-2 rounded bg-white">
                    <div class="wallpaper" >
                        <img :src="movie.image" alt="">
                    </div> 
                    <div class="flex flex-auto flex-col gap-1 bg-white p-3">
                        <h3 class="text-xl">{{ movie.name }}</h3>
                        <div class="flex gap-2">
                            <span class=" bg-blue-600 rounded-full px-3 text-white" v-for="(cat,catIndex) in movie.genres" :key="catIndex">{{ cat }}</span>
                        </div>
                        <p class="mt-2">{{ movie.description }}</p>
                        <div class="flex h-full gap-2 items-end">
                            <small>Rating: ({{ movie.rating }}/5)</small>
                            <div class="flex gap-1">
                                <small 
                                    v-for="(star) in 5" 
                                    :key="star" 
                                    class="cursor-pointer" 
                                    :class="configureClass(star, movie)"
                                    @click="ratingMovie(star, index)"
                                >
                                    &#9733;
                                </small>
                            </div>
                        </div>
                    </div>
                </div>
            </template>
        </div>
    </div>
</template>

<style>
    .wallpaper{
        height: 700px;
        overflow: hidden;
    }
</style>
