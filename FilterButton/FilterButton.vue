<template>
    <div class="flex flex-col">
        <div class="flex flex-row items-center space-x-2">
            <input type="checkbox" v-model="enabled">
            <span>{{filter.name}}</span>
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