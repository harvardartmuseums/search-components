<template>
    <div class="flex flex-col mb-2">
        <div class="flex relative items-center mr-4">
            <input class="opacity-0 absolute left-0 top-0 h-6 w-6" type="checkbox" v-model="enabled" :id="`${query_parameter}-${filter.id}`">
            <div class="bg-white text-black border-2 border-black w-6 h-6 flex flex-shrink-0 justify-center items-center mr-2 focus-within:outline-black">
                <svg class="fill-current hidden w-3 h-3 text-black pointer-events-none" version="1.1" viewBox="0 0 17 12" xmlns="http://www.w3.org/2000/svg">
                    <g fill="none" fill-rule="evenodd">
                    <g transform="translate(-9 -11)" fill="#000" fill-rule="nonzero">
                        <path d="m25.576 11.414c0.56558 0.55188 0.56558 1.4439 0 1.9961l-9.404 9.176c-0.28213 0.27529-0.65247 0.41385-1.0228 0.41385-0.37034 0-0.74068-0.13855-1.0228-0.41385l-4.7019-4.588c-0.56584-0.55188-0.56584-1.4442 0-1.9961 0.56558-0.55214 1.4798-0.55214 2.0456 0l3.679 3.5899 8.3812-8.1779c0.56558-0.55214 1.4798-0.55214 2.0456 0z" />
                    </g>
                    </g>
                </svg>
            </div>
            <label :for="`${query_parameter}-${filter.id}`" class="select-none"><span>{{filter.name}}</span><span v-if="filter.theme">{{`, ${filter.theme}`}}</span></label>
            <div v-if="filter.haschildren">
                <button v-if="filter.children.length !=0" type="button" @click="openChildren = !openChildren">+</button>
            </div>
        </div>
        <div v-if="openChildren">
            <div class="pl-4" v-if="filter.haschildren">
                <filter-button v-for="childFilter in filter.children" v-bind:key="childFilter.id" :filter="childFilter" :query_parameter="query_parameter" :active="checkFilter(childFilter.id)" @filter-enabled="onFilterEnabled" @filter-disabled="onFilterDisabled" :active_filters="active_filters" />
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        props: ['filter', 'active', 'query_parameter', 'active_filters'],
        emits: ['filterEnabled', 'filterDisabled'],
        data() {
            return {
                openChildren: false,
            }
        },
        computed: {
             enabled: {
                get() {
                    return this.active
                    },
                    set(val) {
                        if(val){
                            this.$emit('filterEnabled', {filter_parameter: this.query_parameter, filter_id: this.filter.id, filter_name: this.filter.name})
                        }
                        else{
                            this.$emit('filterDisabled', {filter_parameter: this.query_parameter, filter_id: this.filter.id, filter_name: this.filter.name})
                        }
                        return val
                    }
                }
        },
        methods:{
            onFilterEnabled(data) {
                this.$emit('filterEnabled', data)
            },
            onFilterDisabled(data) {
                this.$emit('filterDisabled', data)
            },
            checkFilter(filter_id){
                var relevant = this.active_filters.find(obj => {
                    return obj.filter_parameter === this.query_parameter && `${obj.filter_value}` == `${filter_id}`
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
        }
</script>

<style lang="postcss">

input:checked + div svg {
  @apply block;
}

</style>