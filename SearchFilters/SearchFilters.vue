<template>
<div v-if="!filter_group_id" class="flex flex-col">
    <filter-group :api_base="api_base" :active_filters="active_filters" :values="null" :flat="false" endpoint="classifications" name="Classifications" query_parameter="classification[]" @filter-selected="onFilterSelected" @filter-removed="onFilterRemoved"/>
    <filter-group :api_base="api_base" :active_filters="active_filters" :values="null" :flat="false" endpoint="worktypes" name="Work Type" query_parameter="worktype[]" @filter-selected="onFilterSelected" @filter-removed="onFilterRemoved"/>
    <filter-group :api_base="api_base" :active_filters="active_filters" :values="null" :flat="false" endpoint="mediumstechniques" name="Technique / Medium" query_parameter="technique[]" @filter-selected="onFilterSelected" @filter-removed="onFilterRemoved"/>
    <filter-group :api_base="api_base" :active_filters="active_filters" :values="null" :flat="false" endpoint="periods" name="Period" query_parameter="period[]" @filter-selected="onFilterSelected" @filter-removed="onFilterRemoved"/>
    <filter-group :api_base="api_base" :active_filters="active_filters" :values="null" flat="places" endpoint="sorted_places" name="Place" query_parameter="place[]" @filter-selected="onFilterSelected" @filter-removed="onFilterRemoved"/>
    <filter-group :api_base="api_base" :active_filters="active_filters" :values="null" :flat="false" endpoint="centuries" name="Century" query_parameter="century[]" @filter-selected="onFilterSelected" @filter-removed="onFilterRemoved"/>
    <filter-group :api_base="api_base" :active_filters="active_filters" :values="null" :flat="false" endpoint="cultures" name="Culture" query_parameter="culture[]" @filter-selected="onFilterSelected" @filter-removed="onFilterRemoved"/>
    <filter-group-galleries :api_base="api_base" :active_filters="active_filters" :values="null" endpoint="sorted_galleries" name="Gallery" query_parameter="gallery[]" @filter-selected="onFilterSelected" @filter-removed="onFilterRemoved"/>
</div>


<div v-if="filtersComplete" class="flex flex-col">
    <filter-group v-for="(group, index) in  filters" :key="index" :active_filters="active_filters" :values="group.values" :endpoint="null" :name="group.name" :query_parameter="`custom[${group.searchparameter}][]`" @filter-selected="onFilterSelected" @filter-removed="onFilterRemoved"/>
</div>
</template>

<script>
import FilterGroup from '../FilterGroup/FilterGroup.vue'
import FilterGroupGalleries from '../FilterGroupGalleries/FilterGroupGalleries.vue'

    export default {
  components: { FilterGroup, FilterGroupGalleries},
        props: ['filter_group_id', 'active_filters', 'params', 'api_base'],
        emits: ['filterSelected', 'filterRemoved'],
        data() {
            return {
                filtersComplete: false,
                filters: [],
            }
        },
        methods: {
            onFilterSelected(data){
                this.$emit('filterSelected', data)
            },
            onFilterRemoved(data){
                this.$emit('filterRemoved', data)
            },
            fetchGroupFilters(){
                let filtersUrl = `${this.api_base}/browse/filters?filter_group=${this.filter_group_id}`
                fetch(`${filtersUrl}`, 
                        {credentials: "same-origin",
                        headers: {
                        'Accept': 'application/json'
                        }
                        })
                .then(res => res.json()
                .then(res => {
                    this.filters = res.custom;
                    this.filtersComplete = true;
			    }
            ))}
        
        },
        mounted() {
            if(this.filter_group_id){
                this.fetchGroupFilters()
            }
        }
    }
</script>