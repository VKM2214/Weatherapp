<template>
  <div class="highlights-item col-md-4 col-sm-6 col-xs-12 border-top">
    <div>
      <div class="card-heading text-center "><b>Wind Status</b> </div>
      <div class="row">
        <div class="col-sm-6 col-md-6 text-center align-middle">
          <p class="card-sub-heading">Angle <img src="../assets/wind.svg" height="40" width="40"></p>
          <p class="card-value">{{highlights.windStatus.derivedWindDirection}}<span>°</span></p>
        </div>
        <div class="col-sm-6 col-md-6 ">
          <p class="card-sub-heading">Wind Speed <img src="../assets/windspeed.svg" height="40" width="40"></p>
          <p class="card-value">{{highlights.windStatus.windSpeed}} km/h</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name:'WindStatus',
    props: ["highlights"],
    components: {},
    data() {
      return {};
    },
    methods: {},
    computed: {}
  };
</script>
