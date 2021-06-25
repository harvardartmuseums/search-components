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
    <div class="flex-none">
        <div class="max-h-56 h-56 overflow-y-auto">
            <div class="flex flex-col text-sm" v-if="showFilterGroup">
                <div class="flex flex-col mt-4">
                    <h4 class="font-bold">Lower Level</h4>
                    <filter-button v-for="filter in filters[0]" v-bind:key="filter.id" :filter="filter" :query_parameter="filter.type == 'medium' ? 'medium[]' : query_parameter" :active="checkFilter(filter)" @filter-enabled="onFilterEnabled" @filter-disabled="onFilterDisabled" :active_filters="active_filters"/>
                </div>
                <div class="flex flex-col mt-4">
                    <h4 class="font-bold">Level 1</h4>
                    <filter-button v-for="filter in filters[1]" v-bind:key="filter.id" :filter="filter" :query_parameter="filter.type == 'medium' ? 'medium[]' : query_parameter" :active="checkFilter(filter)" @filter-enabled="onFilterEnabled" @filter-disabled="onFilterDisabled" :active_filters="active_filters" />
                </div>
                <div class="flex flex-col mt-4">
                    <h4 class="font-bold ">Level 2</h4>
                    <filter-button v-for="filter in filters[2]" v-bind:key="filter.id" :filter="filter" :query_parameter="filter.type == 'medium' ? 'medium[]' : query_parameter" :active="checkFilter(filter)" @filter-enabled="onFilterEnabled" @filter-disabled="onFilterDisabled" :active_filters="active_filters"/>
                </div>
                <div class="flex flex-col mt-4">
                    <h4 class="font-bold">Level 3</h4>
                    <filter-button v-for="filter in filters[3]" v-bind:key="filter.id" :filter="filter" :query_parameter="filter.type == 'medium' ? 'medium[]' : query_parameter" :active="checkFilter(filter)" @filter-enabled="onFilterEnabled" @filter-disabled="onFilterDisabled" :active_filters="active_filters" />
                </div>
                <div class="flex flex-col mt-4">
                    <h4 class="font-bold">Level 4</h4>
                    <filter-button v-for="filter in filters[4]" v-bind:key="filter.id" :filter="filter" :query_parameter="filter.type == 'medium' ? 'medium[]' : query_parameter" :active="checkFilter(filter)" @filter-enabled="onFilterEnabled" @filter-disabled="onFilterDisabled" :active_filters="active_filters" />
                </div>
                <div class="flex flex-col mt-4">
                    <h4 class="font-bold">Level 5</h4>
                    <filter-button v-for="filter in filters[5]" v-bind:key="filter.id" :filter="filter" :query_parameter="filter.type == 'medium' ? 'medium[]' : query_parameter" :active="checkFilter(filter)" @filter-enabled="onFilterEnabled" @filter-disabled="onFilterDisabled" :active_filters="active_filters">
                    </filter-button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import FilterButton from '../FilterButton/FilterButton.vue'

    export default {
        components: {FilterButton},
        props: ['name', 'values', 'query_parameter', 'endpoint', 'active_filters', 'api_base'],
        emits: ['filterSelected', 'filterRemoved'],
        data() {
            return {
                filtersComplete: false,
                filters: [],
                showFilterGroup: false,
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
                    this.filters = res;
                    this.filtersComplete = true;
			    }
            ))},
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
                }
            },
        mounted() {
            if(!this.values){
                this.fetchFilters()
            }
            if(this.values){    
                this.filtersComplete = true
                this.filters = this.values
            }
        }
    }
</script>