<script setup>
    import { watch, ref, reactive, onMounted } from "vue"
    import { Modal } from 'flowbite'
    import { items } from "/src/movies.json"

    import MovieForm from "./MovieForm.vue";

    const movies = ref(items)
    const avgRating = ref(0)
    
    const error = reactive({
        status: false,
        message: "Please confirm error(s)!"
    })
    const modal = ref(null)
    const modifiedBtn = reactive({
        func: "",
        label: ""
    })
    const calculateAvgRating = () => {
        var totRating = 0
        movies.value.map(m => totRating+= m.rating )
        avgRating.value = (totRating / movies.value.length).toFixed(2)
    }
    calculateAvgRating()

/*
 This is an Icon that you can use to represent the stars if you like
 otherwise you could just use a simple ⭐️ emoji, or * character.
*/
// import { StarIcon } from "@heroicons/vue/24/solid";

    const updateRating = (id,rating) => {
        movies.value = movies.value.map((movie) => {
            if (movie.id === id) {
                movie.rating = rating;
            }
            return movie;
        });
    }
    const removeRating = () => {
        movies.value = movies.value.map((movie) => {
            movie.rating = null
            return movie;
        });
    }
    const newMovie = ref(
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
    const validateField = [
        { field: "name", type: "require" , errMsg: "is required!"},
        { field: "description", type: "require" , errMsg: "is required!"},
        { field: "image", type: "require" , errMsg: "is required!"},
        { field: "genres", type: "require" , errMsg: "is required!"},
    ]
    const clearObj = (obj) => {
        for (const key in obj) {
            switch (key) {
                case 'id':
                    obj[key] = null
                    break;
                case 'rating':
                    obj[key] = 0
                    break;
                case 'inTheaters':
                    obj[key] = false
                    break;
                default:
                    obj[key] = ""
                    break;
            }
        }
        return obj
    }
    const modifiedMovie = {
        new: () =>{
            error.status = false
            validate()
            if(!error.status){
                newMovie.value.id = movies.value.length + 1
                movies.value.push(newMovie.value)
                modal.value.hide()
            }
        },
        edit: () => {
            error.status = false
            validate()
            if(!error.status){
                movies.value.map((m,index) => {
                    if(movies.value[index].id == newMovie.value.id){
                        for (const key in newMovie.value) {
                            movies.value[index][key] = newMovie.value[key]
                        }
                    }
                })
                modal.value.hide()
            }
        }
    }
    const addNewMove = () => {
        error.status = false
        newMovie.value = clearObj(newMovie.value)
        modifiedBtn.func = "new"
        modifiedBtn.label = "Create"
        modal.value.show()
    }
    const editMovie = (movieId) => {
        error.status = false
        const tmp = movies.value.filter(m => m.id == movieId)[0]
        Object.assign(newMovie.value, tmp)
        modifiedBtn.func = "edit"
        modifiedBtn.label = "Update"
        modal.value.show()
    }
    const deleteMovie = (movieId) => {
        movies.value.map((m,index) => {
            if(m.id == movieId){
                movies.value.splice(index,1)
            }
        })
    }
    watch(movies.value,()=>{
        calculateAvgRating()
    })
    watch(error, ()=>{
        modifieValidate()
    })
    watch(newMovie.value, ()=>{
        modifieValidate()
    })
    const modifieValidate = () => {
        validateField.map(v => {
            const ele = document.getElementById(v.field)
            if(!newMovie.value[v.field] && error.status){
                ele.innerText = "is required!"
                ele.parentElement.classList.add("error")
            }else{
                ele.innerText = "*"
                ele.parentElement.classList.remove("error")
            }
        })
    }
    const validate = () => {
        validateField.map(v => {
            switch (v.type) {
                case "require":
                    if(newMovie.value[v.field].length==0){
                        error.status = true
                    }
                    break;
                default:
                    break;
            }
        })
    }
    
    onMounted(() => {
        const $modalClose = document.querySelector('#modalClose');
        const $cancelModal = document.querySelector('#cancelModal');
        const modalElement = document.querySelector('#defaultModal');
        const modalOptions = {
            backdropClasses: 'bg-gray-900 bg-opacity-50 dark:bg-opacity-80 fixed inset-0 z-30'
        }
        modal.value = new Modal(modalElement,modalOptions);
        $modalClose.addEventListener('click', () => modal.value.hide());
        $cancelModal.addEventListener('click', () => {
            modal.value.hide()
        });
    })
</script>

<template>
    <!-- This is where your template goes	-->
    <div class="bg-slate-950 py-10">
        <div class="flex sticky px-12 pb-5 shadow top-0 z-40 bg-slate-950">
            <div class="flex gap-2 text-white">
                <span>Total Movie: {{ movies.length }}</span>
                <span> / </span>
                <span>Total Rating: {{ avgRating }}</span>
            </div>
            <div class="ms-auto flex gap-2">
                <button
                    @click="removeRating"
                >
                    Remove Rating
                </button>
                <button
                    @click="addNewMove"
                >
                    Add New Video
                </button>
            </div>
            <div id="defaultModal" tabindex="-1" aria-hidden="true" class="fixed top-0 left-0 right-0 z-40 hidden w-full p-4 overflow-x-hidden overflow-y-auto md:inset-0 h-[calc(100%-1rem)] max-h-full">
                <div class="relative w-full max-w-2xl max-h-full">
                    <!-- Modal content -->
                    <div class="relative bg-slate-600 rounded-lg shadow dark:bg-gray-700">
                        <!-- Modal header -->
                        <div class="flex items-start justify-between p-4 shadow">
                            <h3 class="text-xl font-semibold text-white dark:text-white">
                                Create New Movie
                            </h3>
                            <button id="modalClose" type="button" class="text-gray-400 bg-transparent hover:bg-gray-200 hover:text-gray-900 rounded-lg text-sm p-1.5 ml-auto inline-flex items-center dark:hover:bg-gray-600 dark:hover:text-white">
                                <svg aria-hidden="true" class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z" clip-rule="evenodd"></path></svg>
                                <span class="sr-only">Close modal</span>
                            </button>
                        </div>
                        <!-- Modal body -->
                        <div class="p-6 space-y-6">
                            <div class="flex flex-col gap-2">
                                <span class="label require">
                                    <label> Name </label>
                                    <small id="name" > * </small>
                                </span>
                                <input type="text" v-model="newMovie.name" class="text-black">
                            </div>
                            <div class="flex flex-col gap-2">
                                <span class="label require">
                                    <label> Description </label>
                                    <small id="description" > * </small>
                                </span>
                                <textarea  id="description" cols="30" rows="3" v-model="newMovie.description"></textarea>
                            </div>
                            <div class="flex flex-col gap-2">
                                <span class="label require">
                                    <label> Image </label>
                                    <small id="image" > * </small>
                                </span>
                                <input type="text" v-model="newMovie.image" class="text-black">
                            </div>
                            <div class="flex flex-col gap-2">
                                <span class="label require">
                                    <label> Genres </label>
                                    <small id="genres" > * </small>
                                </span>
                                <select v-model="newMovie.genres" id="genres" @change="validate" multiple>
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
                            <div v-if="error.status" class="text-cetner text-red-400">
                                <small>{{ error.message }}</small>
                            </div>
                        </div>
                        <!-- Modal footer -->
                        <div class="flex items-center p-6 border-t border-slate-700 rounded-b dark:border-gray-600">
                            <button id="cancelModal" type="button" class="cancel">Cancel</button>
                            <button id="update" type="button" class="create ml-auto" @click="modifiedMovie[modifiedBtn.func]">{{ modifiedBtn.label }}</button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="movie-list">
            <MovieForm 
                v-for="(movie, index) in movies" 
                :key="index" 
                :movie="movie" 
                @edit="editMovie"
                @remove="deleteMovie"
                @update:rating="updateRating"
            />
        </div>
    </div>
    
</template>

<style>
    
    /* .label{
        @apply text-white;
    } */
    .error .label{
        @apply text-red-400 flex gap-1 items-center;
    }
    .label.require{
        @apply text-red-400;
    }
    .label.error label{
        @apply text-red-400;
    }
    .movie-list{
        @apply grid grid-cols-3 gap-10 px-10;
    }
</style>
