<template>
  <div>
    <h2 class="text-center">Temperature Variation Chart by Hourly</h2>
     <div class='linechart'><LineChart /></div> 
    </div>
</template>

<script>
import LineChart from './LineChart.vue'
 export default {
    name:'TempVarChart',
    props: ['tempVar','tempVarDt'],
    components: {LineChart},

  }
</script>
<style>
.linechart{
  width: 100%;
}
</style>