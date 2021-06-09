<template>
    <div class="flex space-x-4 justify-center">
        <button class="bg-black text-white p-4" v-for="filter in active_filters" v-bind:key="filter.filter_id" type="button" @click="this.$emit('filterRemoved', {filter_parameter: filter.filter_parameter, filter_id: filter.filter_value, filter_name: filter.filter_name} )">{{filter.filter_name}}  X</button>
    </div>
</template>

<script>
    export default {
        props: ['active_filters','filters'],
        emits: ['filterRemoved'],
    }
</script>