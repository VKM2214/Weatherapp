<template>
  <div class="highlights-item col-md-4 col-sm-6 col-xs-12 border-top">
     <div class="card-heading text-center "><b>UV Index</b></div>
        <div class="text-center pt-3">
      <p>{{highlights.uvIndex}}</p>
    </div>
  </div>
</template>

<script>
  export default {
    name:'UVIndex',
    props: ["highlights"],
    }
</script>
