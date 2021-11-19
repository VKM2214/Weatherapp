<template>
  <div style="position: relative;">
  <temp-var-chart :tempVar="tempVar" :tempVarDt='tempVarDt' ></temp-var-chart> 
    <today-highlights :highlights="highlights"></today-highlights>
  </div>
</template>
<script>
  //import TempVarChart from './TempVarChart.vue';
  import Highlights from './Highlights.vue';
  import TempVarChart from './TempVarChart.vue'
  export default {
    name:'Content',
    props: ['highlights','tempVar','tempVarDt'],
    components: {
   'temp-var-chart': TempVarChart,
      'today-highlights': Highlights
    },
  }
</script>
