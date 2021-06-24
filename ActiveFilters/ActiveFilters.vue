<template>
    <div class="flex justify-center flex-wrap space-x-4">
        <button class="bg-black text-white p-4 flex min-h-0 flex-shrink-0 flex-grow-0 mb-4" v-for="filter in active_filters" v-bind:key="filter.filter_id" type="button" @click="this.$emit('filterRemoved', {filter_parameter: filter.filter_parameter, filter_id: filter.filter_value, filter_name: filter.filter_name} )">
            <span class="leading-none">{{filter.filter_name}}</span>
            <svg class="w-4 h-4 ml-2" xmlns="http://www.w3.org/2000/svg" width="10" height="10" viewBox="0 0 10 10"><path fill="none" stroke="#fff" stroke-miterlimit="10" d="M1 1l8.012 8.012m0-8.012L1 9.012"/></svg>        
        </button>
    </div>
</template>

<script>
    export default {
        props: ['active_filters','filters'],
        emits: ['filterRemoved'],
    }
</script>