<template>
    <div class="weatherapp text-white h-screen" >
        <div class="input-group mb-3">
            <input id="input" type="search"  name="searchWeather" v-model="query"  @keypress="search" placeholder=" Search for places" 
            results="0" class="form-control pac-target-input" autocomplete="off">
            <div data-toggle="tooltip" title="Locate Me!" class="input-group-append">
              <i class="material-icons" @click="searchClick">&#xe55c;</i>
            </div>
        </div>
        
        <div v-if="loading" >
          <Spin />
        </div>

        <div v-if="detail.cod == 404">
           <h1 class=" " >Opps not a valid state ... please enter a valid location 😊 </h1>
        </div>
        
        <div class="side-bar" v-if="detail.cod != undefined && loading == false ">
          <div v-if="this.detail.main != undefined " class="animate__animated animate__zoomIn animate__delay-0s sidecard" >
            <h4 id="temp-head" >  {{ Math.round(detail.main.temp) }} <span>°C</span> </h4>    
            <img v-bind:src="url" class="animate__animated animate__flip animate__delay-1s" alt=""> 
            <h4 class="mt-2" > {{ detail.weather[0].main }} </h4>
            
            <table class="temperature-table table-borderless">
                <thead>
                  <td colspan="4"> Todays Temperature </td>
                </thead>
                <tbody>
                  <tr>
                    <td>Low</td>
                    <td>High</td>
                  </tr>
                  <tr>
                    <td><div class="TodayTemp">
                      {{detail.main.temp_min}}&nbsp;<span>°C</span> 
                      </div>
                      </td>
                    <td><div class="TodayTemp">
                      {{detail.main.temp_max}}<span>°C</span> 
                      </div></td>
                  </tr>
                </tbody>
            </table>
            <h3> {{ detail.name}},  {{ detail.sys.country }}</h3> <br>
            <div v-if="detail.cod != 404">
              <div v-if="loadings">      
                {{ fetchWeather(lat,lon) }}
                <p>{{ timeZone }}</p>
              </div>
            </div>
          </div>   
        </div>                  
    </div>
</template>

<script>

import Spin from './Spin.vue'

export default {
  name:'sidebar',

  data(){
     return{
       location: "lagos,Nigeria",
       api_key: "70542cadca8898ebe0d85b42d854fab7",
       api_key_lat: "70542cadca8898ebe0d85b42d854fab7",
       base_url: "https://api.openweathermap.org/data/2.5/",
       query: "",
       detail: {},
       getDateTimeValue:{},
       url: '',
       loading: false,
       loadings:false,
       lat:'',
       lon:'',
       fetchResponse:{},
       timeZone:''
     }
  },
  methods:{
     
      search(e){
           if ( e.key == "Enter"  ) {
             
                 setTimeout( () => { this.loading = false } , 1000);
                 setTimeout(() =>  { fetch( `${this.base_url}weather?q=${this.query}&units=metric&appid=${this.api_key}`).then(response => response.json()).then( this.results) } , 1000);
          }
            
      },
      searchClick(){
                 setTimeout( () => { this.loading = false } , 10000);
                 setTimeout(() =>  { fetch( `${this.base_url}weather?q=${this.query}&units=metric&appid=${this.api_key}`).then(response => response.json()).then( this.results) } , 1000);

      },
      results(result){
        
        this.detail =  result;
        this.lat =  this.detail.coord.lat;
        this.lon = this.detail.coord.lon;
         this.url = `http://openweathermap.org/img/wn/${this.detail.weather[0].icon}@2x.png`;
        this.loadings = true;
         },
  
      fetchWeather (latitude,longitute) {
          this.lat=latitude;
          this.lon=longitute;
        
          setTimeout(() =>  { fetch(`https://api.openweathermap.org/data/2.5/onecall?lat=${this.lat}&lon=${this.lon}&appid=${this.api_key_lat}`)
          .then(response => response.json()).then(this.fetchResponses) } , 50000);
            setTimeout(() =>  {(90000)});
          console.log('finished')
        },
      
      fetchResponses(fetchresponsevalue){
          this.query='';
          this.fetchResponse =fetchresponsevalue;
          this.timeZone = new Date().toLocaleString("en-US", {timeZone: this.fetchResponse.timezone});
          setTimeout(() =>  {(20000)});
      }
    
  },
 components:{
   Spin
 }

}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@500&display=swap');

#temp-head {
    font-size: 72px;
    margin-right: 1px;
}

.weatherapp{
  font-family: 'Montserrat', sans-serif;

}
.TodayTemp {
    background-color: rgb(0 0 0 / 10%);
    width: 13px;
    padding: 44px;
    padding-right: 92px;
    font-size: 24px;
}
.temperature-table td,.temperature-table tr  {    
  padding: 0.75rem;
    vertical-align: top;
  }
  
#sidebar {
  position: relative;
  display: flex;
  flex-direction: column;
  background-image: linear-gradient(-180deg, #80b6db 0%, #7da7e2 100%);
}

#search {
  text-align: center;
  height: 20vh;
  position: relative;
}

#location-input {
  height: 42px;
  width: 100%;
  opacity: 1;
  border: 0;
  border-radius: 2px;
  background-color: rgba(255, 255, 255, 0.2);
  margin-top: 16px;
  padding-left: 16px;
  color: #ffffff;
  font-size: 1.8rem;
  line-height: 21px;
}

#location-input:focus {
  outline: none;
}

::placeholder {
  color: #FFFFFF;
  opacity: 0.6;
}

#current-weather {
  color: #ffffff;
  font-size: 8rem;
  line-height: 106px;
  position: relative;
}

#current-weather>span {
  color: #ffffff;
  font-size: 3.6rem;
  line-height: 42px;
  vertical-align: super;
  opacity: 0.8;
  top: 15px;
  position: absolute;
}

#weather-desc {
  font-size: 2.0rem;
  color: #ffffff;
  font-weight: 500;
  line-height: 24px;
}

#possibility {
  color: #ffffff;
  font-size: 16px;
  font-weight: 500;
  line-height: 19px;
}

#max-detail,
#min-detail {
  color: #ffffff;
  font-size: 2.0rem;
  font-weight: 500;
  line-height: 24px;
}

#max-detail>i,
#min-detail>i {
  font-style: normal;
  height: 13.27px;
  width: 16.5px;
  opacity: 0.4;
}

#max-detail>span,
#min-detail>span {
  color: #ffffff;
  font-family: Roboto;
  font-size: 1.2rem;
  line-height: 10px;
  vertical-align: super;
}

#max-summary,
#min-summary {
  opacity: 0.9;
  color: #ffffff;
  font-size: 1.4rem;
  line-height: 16px;
  margin-top: 2px;
  opacity: 0.7;
}

#search-btn {
  position: absolute;
  right: 0;
  top: 16px;
  padding: 2px;
  z-index: 999;
  height: 42px;
  width: 45px;
  background-color: rgba(255, 255, 255, 0.2);
  border: none;
}

#dashboard-content {
  text-align: center;
  height: 100vh;
}

#date-desc,
#location-desc {
  color: #ffffff;
  font-size: 1.6rem;
  font-weight: 500;
  line-height: 19px;
  margin-bottom: 15px;
}

#date-desc>img {
  top: -3px;
  position: relative;
  margin-right: 10px;
}

#location-desc>img {
  top: -3px;
  position: relative;
  margin-left: 5px;
  margin-right: 15px;
}

#location-detail {
  opacity: 0.7;
  color: #ffffff;
  font-size: 1.4rem;
  line-height: 20px;
  margin-left: 35px;
}

.centered {
  position: fixed;
  top: 45%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.max-desc {
  width: 80px;
  float: left;
  margin-right: 28px;
}

.temp-max-min {
  margin-top: 40px
}

#dashboard-content {
  background-color: #F7F7F7;
}

span text {
  font-weight: 500 !important;
}

.sidecard{
  margin-top: 160px;
  margin-left:30px;
  position: fixed;
}

@media only screen and (min-width: 768px) {
  #sidebar {
    height: 100vh;
  }

  #info {
    position: fixed;
    bottom: 50px;
    width: 100%;
    padding-left: 15px;
  }

  .wrapper-right {
    margin-top: 80px;
  }
}

@media only screen and (min-width:1440px) {
  #sidebar {
    width: 350px;
    max-width: 350px;
    flex: auto;
  }
  #dashboard-content {
    width: calc(100% - 350px);
    max-width: calc(100% - 350px);
    flex: auto;
  }
}
</style>