























<template>
  <div>
    <h2 class="text-center">Temperature Variation Chart by Hourly</h2>
    <div >
      <LineChart />
    </div>
    <p>hol{{ tempVar }}</p>
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






















<script>
import { Line } from "vue3-chart-v2";

export default {
  name:'LineChart',
  props: ['tempVar','tempVarDt'],
  extends: Line,
  data() {
    return {
      flag:true,
       loaded: false,
      chartData: {
                  labels: [
                    "Falkenberg",
                    "Halmstad",
                    "Hylte",
                    "Kungsbacka",
                    "Laholm",
                    "Varberg",
                  ],

                  datasets: [
                    {
                      label: "Line",
                      data: [],
                      fill: false,
                      tension: 0.1,
                      borderColor: "hotpink",
                      backgroundColor: "hotpink",
                      borderWidth: 2,
                    },
                  ],
      },
      options: {
        responsive: true,
        maintainAspectRatio: false,

        title: {
          display: true,
          text: "Temperature",
          fontColor: "hotpink",
          fontSize: 24,
          position: "left",
          padding: 16,
        },
        legend: {
          align: "end",
          labels: {
            fontColor: "black",
            fontSize: 14,
          },
        },
        layout: {
          padding: 16,
        },
        scales: {
          yAxes: [
            {
              ticks: {
                beginAtZero: true,
              },
              gridLines: {
                display: true,
              },
            },
          ],
          xAxes: [
            {
              gridLines: {
                display: false,
              },
            },
          ],
        },
      },
    };
  },  
 async mounted () {
    this.loaded = false
    try {
      while (this.flag) {
      if (this.tempVarDt!=undefined) {
      this.chartData.datasets[0].data = this.tempVarDt
      this.loaded = true
      this.renderChart(this.chartData, this.options);
      console.log('hello')
      }
      }
    } catch (e) {
      console.error(e)
    }
  }
};
</script>
