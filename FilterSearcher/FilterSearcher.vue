<template>
<input class="focus:outline-black p-2 border-black border-2" v-model="searchTerm" type="search" @input="fuzzySearch"/>
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