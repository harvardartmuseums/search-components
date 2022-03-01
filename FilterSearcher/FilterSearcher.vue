<template>
   <label class="relative">
        <span class="sr-only">Filter Search</span>
        <input class="focus:outline-black w-full border-black border-2 p-2" v-model="searchTerm" type="search" @input="fuzzySearch"/>
                <svg xmlns="http://www.w3.org/2000/svg" width=18 height=24 class="fill-current stroke-current text-hamdarkgray w-auto h-6 absolute top-1/2 transform -translate-y-1/2 right-3" viewBox="0 0 19 22">
                    <path  d="M7.992 2.188c3.2 0 5.805 2.604 5.805 5.805 0 3.2-2.604 5.805-5.805 5.805-3.2 0-5.805-2.604-5.805-5.805 0-3.2 2.605-5.805 5.805-5.805m0-2C3.682.188.187 3.682.187 7.993s3.494 7.805 7.805 7.805 7.805-3.494 7.805-7.805S12.303.188 7.992.188z"/>
                    <path fill="none" stroke-width="3" stroke-miterlimit="10" d="M17.024 20.95l-5.214-7.334"/>
                </svg>
    </label>
</template>

<script>
import Fuse from 'fuse.js'
    export default {
        props: ['filters'],
        emits: ['searchResults'],
        data(){
            return {
                searchTerm: ''
            }
        },
        methods: {
            fuzzySearch(){
                if(this.searchTerm.length == 0){
                    this.$emit('searchResults', {filters: 'reset', limit: 5})
                }
                else {
                    var results = this.fuse.search(this.searchTerm)
                    var set = results.map(i =>  this.filtersClone[i.refIndex])
                    this.$emit('searchResults', {filters: set, limit: results.length - 1})
                    }
        
                },
        },
        created(){
            const options = {
                            ignoreLocation: true,
                            threshold: 0.2,
                            keys: [
                                "name",
                            ]
                        }
            const fuse = new Fuse(this.filters, options)
            this.fuse = fuse
            this.filtersClone = JSON.parse(JSON.stringify(this.filters)) 
        }
        
    }
</script>