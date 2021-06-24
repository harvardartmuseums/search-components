<template>
  <SwitchGroup as="div" class="flex items-center">
    <SwitchLabel as="span" class="mr-3">
      <span class="text-sm font-medium">On View</span>
    </SwitchLabel>
  <Switch v-model="enabled" @click="toggleOnView" :class="[enabled ? 'bg-black' : 'bg-gray-200', 'relative inline-flex flex-shrink-0 h-6 w-11 border-2 border-transparent rounded-full cursor-pointer transition-colors ease-in-out duration-200 focus:outline-black']">
    <span aria-hidden="true" :class="[enabled ? 'translate-x-5' : 'translate-x-0', 'pointer-events-none inline-block h-5 w-5 rounded-full bg-white shadow transform ring-0 transition ease-in-out duration-200']" />
  </Switch>
  </SwitchGroup>
</template>

<script>
import { ref, watch} from 'vue'
import { Switch, SwitchGroup, SwitchLabel } from '@headlessui/vue'

export default {
  components: {
    Switch,
    SwitchGroup,
    SwitchLabel,
  },
  emits: ['filterSelected', 'filterRemoved'],
  props: {
        active: Boolean
  },
  setup(props) {
    
    const enabled = ref(props.active)
    
    watch(() => props.active, (active, prevActive) => { 
        enabled.value = active
    })
    return {
      enabled,
    }
  },
  methods: {
      toggleOnView(){
          if(this.enabled){
              this.$emit('filterSelected', {filter_parameter: 'onview', filter_id: '1', filter_name: 'On View'})
          }
          else{
              this.$emit('filterRemoved', {filter_parameter: 'onview', filter_id: '1', filter_name: 'On View'})
          }
      }

  },
}
</script>