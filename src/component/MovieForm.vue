<script setup>
    const {movie} = defineProps({
        movie: { type: Object, default: null },
    });
    const emit = defineEmits(["edit", "remove", "update:rating"])
    const updateRating = (rating) => {
        emit("update:rating", movie.id, rating);
    }

    const removeMovie = () =>  {
        emit("remove", movie.id);
    }

    const editMovie = () =>  {
        emit("edit", movie.id);
    }
    console.log("Moveis ", movie)
</script>

<template>
    <div class="flex flex-col gap-2 rounded bg-white">
        <div class="wallpaper relative" >
            <img :src="movie.image" alt="">
            <div v-if="movie.rating" class="absolute top-[1rem] right-10">
                <div class="rating-star relative">
                    <span class="star text-orange-300 text-6xl">&#9733;</span>
                    <span class="number text-white">{{ movie.rating }}</span>
                </div>
            </div>
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
                        :class="[
                            star <= movie.rating ? 'text-orange-300' : 'text-gray-300',
                            star === movie.rating ? 'cursor-not-allowed' : 'cursor-pointer'
                        ]"
                        
                        @click="updateRating(star)"
                    >
                        &#9733;
                    </small>
                </div>
                <div class="ms-auto flex gap-2">
                    <button @click="editMovie()">Edit</button>
                    <button @click="removeMovie()">Delete</button>
                </div>
            </div>
        </div>
    </div>
</template>

<style>
    .wallpaper{
        height: 380px;
        overflow: hidden;
    }
    .rating-star .number{
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
    }
</style>