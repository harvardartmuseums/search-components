<template>
    <div class="w-full">
        <div class="flex flex-col items-center justify-center">
            <label class="relative w-full md:w-3/4 leading-none mb-2 md:mb-4" ref="searchbox">
                <span class="sr-only">Collections Search</span>
                <input v-model="input" type="search" :placeholder="placeholder" class="w-full border-black border-4 h-12 p-2 md:p-4 focus:outline-black">
                        <svg xmlns="http://www.w3.org/2000/svg" width=18 height=24 class="fill-current stroke-current text-hamdarkgray w-auto h-6 absolute top-1/2 transform -translate-y-1/2 right-3" viewBox="0 0 19 22">
                            <path  d="M7.992 2.188c3.2 0 5.805 2.604 5.805 5.805 0 3.2-2.604 5.805-5.805 5.805-3.2 0-5.805-2.604-5.805-5.805 0-3.2 2.605-5.805 5.805-5.805m0-2C3.682.188.187 3.682.187 7.993s3.494 7.805 7.805 7.805 7.805-3.494 7.805-7.805S12.303.188 7.992.188z"/>
                            <path fill="none" stroke-width="3" stroke-miterlimit="10" d="M17.024 20.95l-5.214-7.334"/>
                        </svg>
            </label>
            <div class="w-full relative text-xs md:w-3/4 flex justify-between items-center mb-4">
                <button :class="showFilters ? 'border-t-2 shadow-inner-dark border-b-0' : 'border-b-4'" class="text-left font-bold h-11 bg-gray-500 rounded-sm text-white py-2 px-4  border-gray-700 text-sm focus:outline-black" type="button" @click="toggleFilters">Filters</button>
                <div class="flex flex-grow justify-center">
                    <span class="text-gray-500 text-xs md:text-base text-center">Showing {{formattedTotal}} works</span>
                </div>

                <button v-if="suppress_on_view" class="md:w-28 text-right" type="button" @click="resetSearch">Reset</button>
                <on-view-toggle @filter-selected="onFilterSelected" @filter-removed="onFilterRemoved" v-else :active="onView" />

                <transition name="slide">
                <div v-if="showFilters" class="flex md:hidden flex-col shadow-xl w-full z-40 absolute left-0 top-0 bg-white">
                    <div class="flex h-6 w-full justify-end items-center">
                        <div class="justify-center">
                            <button class="px-2 py-4" type="button"  @click="toggleFilters">
                                <svg class="w-4 h-4 ml-2" xmlns="http://www.w3.org/2000/svg" width="10" height="10" viewBox="0 0 10 10"><path fill="none" stroke="#000" stroke-miterlimit="10" d="M1 1l8.012 8.012m0-8.012L1 9.012"/></svg><span class="sr-only">Close Filters Menu</span></button>
                        </div>
                    </div>
                    <div class="px-6 pb-6">      
                        <search-filters :api_base="api_base" :active_filters="activeFilters" @filter-selected="onFilterSelected" @filter-removed="onFilterRemoved" :filter_group_id="filter_group_id" :params="params"/>
                    </div>
                </div>
            </transition>
            </div>
            <active-filters :active_filters="activeFilters" @filter-removed="onFilterRemoved" class="md:mb-4 md:w-3/4 flex-shrink" />
        </div>

        <!-- Fixed seach interface -->
        <transition name="fade">
        <div v-if="offscreen" class="w-screen bg-white fixed top-0 left-0 z-600 px-4 pt-4 md:px-0 md:pb-8 md:pt-12 shadow-xl">
            <div class="flex flex-col items-center justify-center">
                <label class="relative w-full md:w-3/4 leading-none mb-2 md:mb-4">
                    <span class="sr-only">Collections Search</span>
                    <input v-model="input" type="search" :placeholder="placeholder" class="w-full border-black border-4 h-12 p-2 md:p-4 focus:outline-black">
                            <svg xmlns="http://www.w3.org/2000/svg" width=18 height=24 class="fill-current stroke-current text-hamdarkgray w-auto h-6 absolute top-1/2 transform -translate-y-1/2 right-3" viewBox="0 0 19 22">
                                <path  d="M7.992 2.188c3.2 0 5.805 2.604 5.805 5.805 0 3.2-2.604 5.805-5.805 5.805-3.2 0-5.805-2.604-5.805-5.805 0-3.2 2.605-5.805 5.805-5.805m0-2C3.682.188.187 3.682.187 7.993s3.494 7.805 7.805 7.805 7.805-3.494 7.805-7.805S12.303.188 7.992.188z"/>
                                <path fill="none" stroke-width="3" stroke-miterlimit="10" d="M17.024 20.95l-5.214-7.334"/>
                            </svg>
                </label>
                <div class="w-full relative text-xs md:w-3/4 flex justify-between items-center mb-4">
                    <button :class="showFilters ? 'border-t-2 shadow-inner-dark border-b-0' : 'border-b-4'" class="text-left font-bold h-11 bg-gray-500 rounded-sm text-white py-2 px-4  border-gray-700 text-sm focus:outline-black" type="button" @click="toggleFilters">Filters</button>
                    <transition name="slide">
                        <div v-if="showFilters" :class="{'hidden':!offscreen, 'hidden md:flex':offscreen}" class="flex-col z-40 absolute left-0 top-11 bg-white h-max-content shadow-xl">
                            <div class="px-4 py-4">      
                                <search-filters :api_base="api_base" :active_filters="activeFilters" @filter-selected="onFilterSelected" @filter-removed="onFilterRemoved" :filter_group_id="filter_group_id" :params="params"/>
                            </div>
                        </div>
                    </transition>
                    <div class="flex flex-grow justify-center">
                        <span class="text-gray-500 text-xs md:text-base text-center">Showing {{formattedTotal}} works</span>
                    </div>

                    <button v-if="suppress_on_view" class="md:w-28 text-right" type="button" @click="resetSearch">Reset</button>
                    <on-view-toggle @filter-selected="onFilterSelected" @filter-removed="onFilterRemoved" v-else :active="onView" />

                    <transition name="slide">
                    <!--Mobile Filters fixed -->
                    <div v-if="showFilters" class="flex md:hidden flex-col shadow-xl w-full z-40 absolute left-0 top-0 bg-white">
                        <div class="flex h-6 w-full justify-end items-center">
                            <div class="justify-center">
                                <button class="px-2 py-4" type="button"  @click="toggleFilters">
                                    <svg class="w-4 h-4 ml-2" xmlns="http://www.w3.org/2000/svg" width="10" height="10" viewBox="0 0 10 10"><path fill="none" stroke="#000" stroke-miterlimit="10" d="M1 1l8.012 8.012m0-8.012L1 9.012"/></svg><span class="sr-only">Close Filters Menu</span></button>
                            </div>
                        </div>
                        <div class="px-6 pb-6">      
                            <search-filters :api_base="api_base" :active_filters="activeFilters" @filter-selected="onFilterSelected" @filter-removed="onFilterRemoved" :filter_group_id="filter_group_id" :params="params"/>
                        </div>
                    </div>
                    </transition>
                </div>
                <active-filters :active_filters="activeFilters" @filter-removed="onFilterRemoved" class="md:mb-4 md:w-3/4 flex-shrink" />
            </div>
        </div>
        </transition>
        <!-- End fixed interface -->
       
        <div class="w-full flex justify-center pb-8">
            <transition name="slide">
            <div v-if="showFilters" :class="{'md:flex':!offscreen}" class="hidden flex-col pt-5 pb-4 w-72">
       
                <search-filters :api_base="api_base" :active_filters="activeFilters" @filter-selected="onFilterSelected" @filter-removed="onFilterRemoved" :filter_group_id="filter_group_id" :params="params"/>
           
            </div>
            </transition>
            
            <div class="flex flex-grow flex-col">
                    <div class="w-full flex justify-center">
                        <div v-masonry transition-duration="0.3s" item-selector=".item" fit-width="true">
                            <div v-masonry-tile class="item px-8 mb-4 md:mb-0 p-8 w-full md:w-80" v-for="(item, index) in results" v-bind:key="index">
                                <search-item :item="item" />
                            </div>
                        </div>
                    </div>
                
                <div v-if="totalRecords > results.length" class="w-full flex justify-center mt-4">
                    <button type="button" class="p-4 bg-black text-white focus:outline-black" @click="appendResults">Load More</button>    
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import debounce from 'lodash/debounce'
import SearchFilters from '../SearchFilters/SearchFilters.vue'
import ActiveFilters from '../ActiveFilters/ActiveFilters.vue'
import SearchItem from '../SearchItem/SearchItem.vue'
import OnViewToggle from '../OnViewToggle/OnViewToggle.vue'

    export default {
        components: {SearchFilters, ActiveFilters, SearchItem, OnViewToggle},
        props: ['context', 'url', 'filter_group_id', 'placeholder', 'suppress_on_view'],
        data() {
            return {
                results: [],
                searching: false,
                noResults: true,
                params: [],
                activeFilters: [],
                filterNames:[],
                offset: 0,
                loadAmount: 12,
                totalRecords: 0,
                input: '',
                showFilters: false,
                onView: false,
                offscreen: false,
            }
        },
        computed: {
            formattedTotal(){
                return Intl.NumberFormat().format(this.totalRecords)
            },
            api_base(){
                if (typeof process !== 'undefined'){
                    return process.env.MIX_API_BASE
                }
                else {
                    return import.meta.env.VITE_API_BASE
                }
            }
        },
        methods: {
            handleScroll (event) {
                let bounding = this.$refs.searchbox.getBoundingClientRect()
                if (bounding.top < 55){
                    this.offscreen = true;
                }
                else {
                    this.offscreen = false;
                }
            },
            resetSearch(){
                this.input = ''
                this.offset = 0
                this.activeFilters.forEach(filter => this.onFilterRemoved({filter_parameter: filter.filter_parameter, filter_id: filter.filter_value, filter_name: filter.filter_name}));
            },
            onFilterSelected(data) { 
                var param = this.params.find(obj => obj.key == data.filter_parameter)
                if(typeof param !== "undefined"){
                    param.value = `${param.value}|${data.filter_id}`
                }
                else {
                    this.params.push({key: data.filter_parameter, value: data.filter_id})
                }
                this.activeFilters.push({filter_parameter: data.filter_parameter, filter_value: data.filter_id, filter_name: data.filter_name}) 
                console.log("filter added")
                console.log(this.activeFilters)
                this.offset = 0
                if(data.filter_parameter == "onview"){
                        this.onView = true
                    }
                this.search()
            },
            onFilterRemoved(filter){
                    console.log(`onFilterRemoved:`)
                    console.log(filter)
                    console.log(this.params)
                    var objIndex = this.params.findIndex((obj => obj.key == filter.filter_parameter))
                    console.log("active filters:")
                    console.log(this.activeFilters)
                    var activeIndex = this.activeFilters.findIndex((obj => {
                            return obj.filter_parameter == filter.filter_parameter && `${obj.filter_value}` == `${filter.filter_id}`
                        }));
                    console.log(`activeIndex = ${activeIndex}`)
                    var values = `${this.params[objIndex].value}`.split('|')
                    console.log(this.activeFilters)
                    if(values.length == 1){
                        this.params.splice(objIndex, 1)
                    }
                    else{
                        var arr = values.filter(x=>x!=filter.filter_id)
                        this.params[objIndex].value = arr.join('|')
                    }
                    this.activeFilters.splice(activeIndex, 1)
                    this.offset = 0
                    if(filter.filter_parameter == "onview"){
                        this.onView = false
                    }
                    this.search()
            },
            appendResults(){
                this.offset = this.offset + this.loadAmount
                this.search(null, true)
            },
            search: debounce(function(savedParams = null, appendResults = null){
                this.searching = true;
                if(!savedParams){
                    var parameters = new URLSearchParams(`q=${this.input}`)
                    this.params.forEach(function(item){
                        parameters.append(item.key, item.value)
                    })
                }
                else{
                    var parameters = savedParams
                }
                parameters.append('load_amount', this.loadAmount)
                parameters.append('offset', this.offset)
                let searchUrl = `${this.api_base}/browse?${parameters.toString()}`
                fetch(`${searchUrl}`, 
                        {credentials: "same-origin",
                        headers: {
                        'Accept': 'application/json'
                        }
                        })
                .then(res => res.json()
                .then( res => {
                    this.searching = false;
                    this.totalRecords = res.info.totalrecords;
                    this.noResults = res.info.totalrecords === 0;
                    if(appendResults){
                        this.results = this.results.concat(res.records);
                        setTimeout(() =>{ this.$redrawVueMasonry() }, 750)
                    }
                    else {
                        this.results = res.records
                        setTimeout( () => { this.$redrawVueMasonry() }, 750)
                    }
			    }
            ))}, 500),
        toggleFilters(){
            this.showFilters = !this.showFilters
            setTimeout( () => { this.$redrawVueMasonry() }, 750)
        }
        },
        watch: {
            input: function() {
                this.offset = 0
                this.params = []
                this.search()
            },
            params: function() {
                this.search();
            }
        },
        created () {
            window.addEventListener('scroll', this.handleScroll);
        },
        destroyed () {
            window.removeEventListener('scroll', this.handleScroll);
        },
        mounted() {
            if(this.url){
                var queryString = this.url.substring(this.url.indexOf("?"))
                if(this.filter_group_id){
                  queryString = `${queryString}&filter_group=${this.filter_group_id}&group=${this.filter_group_id}`  
                }
                let importParams = new URLSearchParams(queryString)
                importParams.forEach(function(value, key) {
                    if(key == 'topic[]'){
                        importParams.set('custom[topic][]', value)
                        importParams.delete(key)
                    }
                });
                const entries = importParams.entries()
                fetch(`${this.api_base}/browse/filterlookup${queryString}`, 
                        {credentials: "same-origin",
                        headers: {
                        'Accept': 'application/json'
                        }
                        })
                .then(res => res.json()
                .then( res => {
                    this.filterNames = res
                    console.log("filterNames:");
                    console.log(this.filterNames)
                    for(const entry of entries){
                        this.params.push({key: entry[0], value: entry[1]}) 
                        if(entry[0] !== 'load_amount' && entry[0] !=='q' && entry[0] !== 'load_offset'){
                            const values = entry[1].split('|');
                            for(const value of values){
                                const name = this.filterNames.find(obj => entry[0].includes(obj.filter_parameter) && obj.filter_id == value)
                                if(typeof name !== 'undefined'){
                                    console.log('activeFilters:')
                                    console.log(this.activeFilters)
                                    this.activeFilters.push({filter_parameter: entry[0], filter_value: value, filter_name: name.filter_name})
                                }
                            }
                        }
                    }
                    this.search(importParams)
			    }))
            }
            else{
                this.search()
            }
        }
    }
</script>

<style>
 .slide-enter-active{
  animation: menu-slide .5s;
}

.slide-leave-active {
  animation: menu-slide .5s reverse;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.75s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

@keyframes menu-slide {
  from {
    transform: translateX(-100%);
  }
  to {
    transform: translateX(0);
  }
}
</style>