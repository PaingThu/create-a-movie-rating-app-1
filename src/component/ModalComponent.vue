<script setup>
    import { onMounted, reactive, ref } from 'vue'
    import { initFlowbite } from 'flowbite'
    import { movies } from "/src/controller.js"
    const newMovie = reactive(
        {
			"id": null,
			"name": "",
			"description": "",
			"image": "",
			"rating": 0,
			"genres": [],
			"inTheaters": false
		},
    )
    const error = ref([])

    const requiredFields = [
        "name",
        "genres"
    ]
    const addNewMovie = () => {
        error.value = []
        validate()
        if(error.value.length == 0){
            console.log("New Movie ", newMovie)
            newMovie.id = movies.vlaue.length + 1
            console.log("newMovie ",newMovie)
            movies.vlaue.push({id: movies.vlaue.length + 1, name: newMovie.name})
            console.log("Items ", movies.value)
        }
    }
    const validate = () => {
        requiredFields.map(rf => {
            console.log("v ", newMovie[rf])
            if(!newMovie[rf]){
                error.value.push(rf)
            }
        })
        console.log("Error ", error.value)
    }

    // initialize components based on data attribute selectors
    onMounted(() => {
        initFlowbite();
    })
</script>
<template>
    <button
        data-modal-toggle="defaultModal" 
        data-modal-target="defaultModal"
        class="ms-auto"
    >
        Add New Video
    </button>
    <div id="defaultModal" tabindex="-1" aria-hidden="true" class="fixed top-0 left-0 right-0 z-50 hidden w-full p-4 overflow-x-hidden overflow-y-auto md:inset-0 h-[calc(100%-1rem)] max-h-full">
        <div class="relative w-full max-w-2xl max-h-full">
            <!-- Modal content -->
            <div class="relative bg-slate-600 rounded-lg shadow dark:bg-gray-700">
                <!-- Modal header -->
                <div class="flex items-start justify-between p-4 shadow">
                    <h3 class="text-xl font-semibold text-white dark:text-white">
                        Create New Movie
                    </h3>
                    <button type="button" class="text-gray-400 bg-transparent hover:bg-gray-200 hover:text-gray-900 rounded-lg text-sm p-1.5 ml-auto inline-flex items-center dark:hover:bg-gray-600 dark:hover:text-white" data-modal-hide="defaultModal">
                        <svg aria-hidden="true" class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z" clip-rule="evenodd"></path></svg>
                        <span class="sr-only">Close modal</span>
                    </button>
                </div>
                <!-- Modal body -->
                <div class="p-6 space-y-6">
                    <div class="flex flex-col gap-2">
                        <label>Name</label>
                        <input type="text" v-model="newMovie.name" class="text-black">
                    </div>
                    <div class="flex flex-col gap-2">
                        <label>Description</label>
                        <textarea  id="description" cols="30" rows="3" v-model="newMovie.description"></textarea>
                    </div>
                    <div class="flex flex-col gap-2">
                        <label>Image</label>
                        <input type="text" v-model="newMovie.image" class="text-black">
                    </div>
                    <div class="flex flex-col gap-2">
                        <label>Genres</label>
                        <select v-model="newMovie.genres" id="genres" multiple>
                            <option>Action</option>
                            <option>Comedy</option>
                            <option>Horror</option>
                            <option>Sci-Fi</option>
                            <option>Western</option>
                            <option>Romance</option>
                            <option>Thriller</option>
                            <option>Fantasy</option>
                        </select>
                    </div>
                    <div class="flex gap-2 items-center">
                        <input type="checkbox" id="inTheaters" v-model="newMovie.inTheaters">
                        <label for="inTheaters">In threater</label>
                    </div>
                </div>
                <!-- Modal footer -->
                <div class="flex items-center p-6 border-t border-slate-700 rounded-b dark:border-gray-600">
                    <button data-modal-hide="defaultModal" type="button" class="cancel">Cancel</button>
                    <button data-modal-hide="defaultModal" type="button" class="create ml-auto" @click="addNewMovie">Create</button>
                </div>
            </div>
        </div>
    </div>
</template>

<style>
</style>