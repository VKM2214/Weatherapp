<template>
  <div class="highlights-item col-md-4 col-sm-6 col-xs-12 border-left border-right border-top">
    <div>
      <div class="card-heading text-center  "><b>Visibility in m/hr</b></div>
    <div v-if="highlights.visibility < 10000">
      <meter v-bind:value="highlights.visibility" min="0" max="10000" v-bind:title="highlights.visibility">
       <span class="meter-value"><p>0.5491</p></span></meter>
      </div>
     <div v-else>
      <meter v-bind:value="highlights.visibility" min="0" max="30000" v-bind:title="highlights.visibility"></meter>
      </div>
      <p>{{}}</p>
    </div>
  </div>
</template>

<script>
  export default {
    name:'Visibility',
    props: ["highlights"],
    components: {},
    methods: {},
    computed: {},
    data() {
      return {
        type: "hlineargauge",
        width: "100%",
        height: "100%",
        containerbackgroundopacity: 0,
        dataformat: "json",
        creditLabel: false,
        datasource: {
          chart: {
            caption: "Air Visibility",
            captionFontBold: "0",
            captionFontColor: "#000000",
            baseFont: "Roboto",
            numberSuffix: " km",
            lowerLimit: "0",
            upperLimit: "40",
            showPointerShadow: "1",
            animation: "1",
            transposeAnimation: "1",
            theme: "fusion",
            bgAlpha: "0",
            canvasBgAlpha: "0",
            valueFontSize: "20",
            valueFontColor: "#000000",
            valueFontBold: "1",
            pointerBorderAlpha: "0",
            chartBottomMargin: "40",
            captionPadding: "30",
            chartTopMargin: "30"
          },
          colorRange: {
            color: [
              {
                minValue: "0",
                maxValue: "4",
                label: "Fog",
                code: "#6297d9"
              },
              {
                minValue: "4",
                maxValue: "10",
                label: "Haze",
                code: "#7DA9E0"
              },
              {
                minValue: "10",
                maxValue: "40",
                label: "Clear",
                code: "#D8EDFF"
              }
            ]
          },
          pointers: {
            pointer: [
              {
                //value: this.highlights.visibility.toString()
              }
            ]
          }
        }
      };
    },
    watch: {
      highlights: {
        handler: function() {
         // this.datasource.pointers.pointer[0].value = this.highlights.visibility.toString();
        },
        deep: true
      }
    }
  };


</script>

<style>
.meter-value {
  display: block; width: 50px; height: 20px;
}
meter{
    display: block;
    border: 1px outset;
    height: 50px;
    width: 250px;
    overflow: hidden;
    align-content: center;
    position: absolute;
    margin-top: 10px;
}
meter div
{
    display: block;
    border-right:1px solid #000;
    height: 20px;
    background: #b4e391; /* old browsers */
    background: -moz-linear-gradient(top, #b4e391 0%, #672c97 35%, #b4e391 100%); /* firefox */
    background: -webkit-gradient(linear, left top, left bottom, color-stop(0%,#b4e391), color-stop(35%,#44AA00), color-stop(100%,#b4e391)); /* webkit */
    filter: progid:DXImageTransform.Microsoft.gradient( startColorstr='#b4e391', endColorstr='#b4e391',GradientType=0 ); /* ie */
}
.meterValueTooHigh div, .meterValueTooLow div{
    background: #ffd65e; /* old browsers */
    background: -moz-linear-gradient(top, #ffd65e 0%, #FBFF47 35%, #febf04 100%); /* firefox */
    background: -webkit-gradient(linear, left top, left bottom, color-stop(0%,#ffd65e), color-stop(35%,#FBFF47), color-stop(100%,#febf04)); /* webkit */
    filter: progid:DXImageTransform.Microsoft.gradient( startColorstr='#ffd65e', endColorstr='#febf04',GradientType=0 ); /* ie */
}
.meterIsMaxed
{
    border-right: 0px none !important;
}
.meter-guage-row{

}
</style>
