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
       arrat:[],
      chartData: {
                  labels: [
                    "10:00 am",
                    "11:00 am",
                    "12:00 pm",
                    "13:00 pm",
                    "14:00 pm",
                    "15:00 pm",
                    "16:00 pm",
                    "17:00 pm",
                  ],

                  datasets: [
                    {
                      label: "Temp °F",
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
      this.array = Array(10).fill().map(() => Math.floor(300 * Math.random()));
      this.chartData.datasets[0].data = this.array
      this.renderChart(this.chartData, this.options);
  }
};
</script>
