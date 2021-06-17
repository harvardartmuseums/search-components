<template>
    <div class="flex items-center">
        <h3 class="font-bold text-xl">{{name}}</h3>
             <button type="button" @click="showFilterGroup = !showFilterGroup"
                        class="flex items-center dark:text-light focus:outline-black"
                        role="button"
                        aria-haspopup="true"
                        :aria-expanded="showFilterGroup ? 'true' : 'false'"
                    >
                    <span aria-hidden="true" class="ml-auto">
                    <!-- active class 'rotate-180' -->
                    <svg
                        class="w-8 transition-transform transform"
                        :class="{ 'rotate-180': showFilterGroup }"
                        xmlns="http://www.w3.org/2000/svg"
                        fill="none"
                        viewBox="0 0 24 24"
                        stroke="currentColor"
                    >
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7" />
                    </svg>
                    </span>
                </button>
    </div>
    <div class="flex flex-col" v-if="showFilterGroup">
        <filter-searcher v-if="filtersComplete" @search-results="onSearchResults" :filters="filters" class="my-2"/>
        <div class="flex-none">
            <div class="max-h-48 h-48 overflow-y-auto">
                <filter-button v-for="filter in results"  v-bind:key="filter.id" :filter="filter" :query_parameter="filter.type == 'medium' ? 'medium[]' : query_parameter" :active="checkFilter(filter)" @filter-enabled="onFilterEnabled" @filter-disabled="onFilterDisabled" :active_filters="active_filters" />
            </div>
        </div>
    </div>
</template>

<script>
import FilterButton from '../FilterButton/FilterButton.vue'
import FilterSearcher from '../FilterSearcher/FilterSearcher.vue'

    export default {
        components: {FilterButton, FilterSearcher},
        props: ['name', 'values', 'query_parameter', 'endpoint', 'active_filters', 'api_base', 'flat'],
        emits: ['filterSelected', 'filterRemoved'],
        data() {
            return {
                filtersComplete: false,
                results: [],
                showFilterGroup: false,
                limit: 5,
            }
        },
        methods: {
            onFilterEnabled(data){
                this.$emit('filterSelected', data)
            },
            onFilterDisabled(data){
                this.$emit('filterRemoved', data)
            },
            fetchFilters(){
                let filtersUrl = `${this.api_base}/browse/filters/${this.endpoint}`
                fetch(`${filtersUrl}`, 
                        {credentials: "same-origin",
                        headers: {
                        'Accept': 'application/json'
                        }
                        })
                .then(res => res.json()
                .then(res => {
                    this.filters = Object.keys(res).map(key => res[key]);
                    this.results = JSON.parse(JSON.stringify(this.filters));
                    if(this.flat){
                        let flatFiltersUrl = `${this.api_base}/browse/filters/${this.flat}`
                        fetch(`${flatFiltersUrl}`, 
                                {credentials: "same-origin",
                                headers: {
                                'Accept': 'application/json'
                                }
                                })
                        .then(res => res.json()
                        .then(res => {
                            this.flatFilters = Object.keys(res).map(key => res[key]);
                            this.filtersComplete = true;
                    })
                )}
                else {
                    this.filtersComplete = true;
                }
			    }
            ))
            
             
            },
            checkFilter(filter){
                var relevant = this.active_filters.find(obj => {
                    if(filter.type == 'medium'){
                        return obj.filter_parameter === 'medium[]' && `${obj.filter_value}` == `${filter.id}`
                    }
                    else {
                        return obj.filter_parameter === this.query_parameter && `${obj.filter_value}` == `${filter.id}`
                    }
                }
                )
                if(typeof relevant != 'undefined'){
                            return true
                        }
                        else {
                            return false
                        }
                },
            onSearchResults({filters, limit}){
                this.limit = limit
                if(filters == 'reset'){
                    this.filtersComplete = false
                    this.results = JSON.parse(JSON.stringify(this.filters))
                    this.filtersComplete = true
                }
                else {
                    console.log('onsearchresults')
                    console.log(filters)
                    this.results = filters
                }
            },
            showAll(){
                this.filtersComplete = false
                this.results = JSON.parse(JSON.stringify(this.filters))
                this.limit = this.results.length - 1
                this.filtersComplete = true
            }
            },
        mounted() {
            if(!this.values){
                this.fetchFilters()
            }
            if(this.values){    
                this.filtersComplete = true
                this.filters = this.values
                this.results = this.values
            }
        },
    }
</script>