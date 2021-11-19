<template>
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css"/>
    <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.css">
    <link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">



    <div class="row">
        <div id="sidebarmain" class="col-md-3 col-sm-4 col-xs-12 sidebarmain">
            <div class="weatherapp text-white h-screen" >
                <div class="input-group">
                    <input id="location-input" type="search"  name="searchWeather" v-model="query" 
                     @keypress="search" placeholder="Search" 
                    results="0" class="form-control" autocomplete="off">
                    <div data-toggle="tooltip" id="search-btn" title="Locate Me!" class="input-group-append">
                    &nbsp;<i style="font-size:24px" class="fa">&#xf041;</i>
                    </div>
                </div>
            
            <div v-if="loading" >
            <Spin />
            </div>

            <div v-if="detail.cod == 404">
            <h1 class=" " >Opps not a valid state ... please enter a valid location 😊 </h1>
            </div>
            
            <div v-if="detail.cod != undefined && loading == false ">
            <div v-if="this.detail.main != undefined " class="animate__animated animate__zoomIn animate__delay-0s sidecard" >
                
            <img v-bind:src="url" class="animate__animated animate__flip animate__delay-1s" alt=""> {{ detail.weather[0].main }}
             <div class="wrapper-left"> 
              <div id="current-weather">
                {{ Math.round(detail.main.temp) }}
                <span>°C</span>
              </div><br>
                <div class="min-desc">
                <div id="min-detail">
                   <h4>Min &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Max</h4>
                   {{detail.main.temp_min}}
                    °C,
                    {{detail.main.temp_max}}
                    °C
                  </div>
                </div>
                 </div>
             <div class="wrapper-right">
              <div class="date-time-info">
                <div id="date-desc">
                  <img src="./assets/calendar.svg" width="15" height="20">
                  {{ timeZone }}
                </div>
              </div>
              <div class="location-info">
                <div id="location-desc">
                  <img
                    src="./assets/location.svg"
                    width="8"
                    height="15.83"
                    style="opacity: 0.9;"
                  >
                  {{ detail.name}},  {{ detail.sys.country }}

                </div>
              </div>
            </div>
            </div>   
            </div>                  
        </div>
        </div>
  
            <Content class="col-md-9 col-sm-8 col-xs-12 content" id="content" :highlights="highlights" :tempVarDt="tempVarDt" :tempVar ="tempVar" ></Content>           
    </div>

</template>

<script> 
import Spin from './components/Spin.vue'
import Content from './components/Content.vue'

export default({
    name: 'App',
    components:{
    Spin,Content
    },
    data(){
       return{ highlights: { uvIndex: '',   visibility: '',  windStatus: { windSpeed: '',derivedWindDirection: '' } },
       location: "",
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
       timeZone:'',
       tempVar:[],
       tempVarDt:[],
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
         setTimeout(() =>  { fetch(`https://api.openweathermap.org/data/2.5/onecall?lat=${this.lat}&lon=${this.lon}&appid=${this.api_key_lat}`)
         .then(response => response.json()).then(this.fetchResponses) } , 1000);
        
         },
  
          
      fetchResponses(fetchresponsevalue){
          
          this.fetchResponse =fetchresponsevalue;
       
          this.timeZone = new Date().toLocaleString("en-US", {timeZone: this.fetchResponse.timezone});
          this.organizeTodayHighlights();
        },
      organizeTodayHighlights: function() {
        this.highlights.uvIndex = this.fetchResponse.current.uvi;
     this.highlights.visibility = this.fetchResponse.current.visibility;
     this.highlights.windStatus.windSpeed = this.fetchResponse.current.wind_speed;
     this.highlights.windStatus.derivedWindDirection = this.fetchResponse.current.wind_deg;
       console.log('here comes',this.temp)
      for (var i = 0; i < 8; i++) {
     this.tempVar.push(this.fetchResponse.hourly[i].dt); //;
      this.tempVarDt.push(this.fetchResponse.hourly[i].temp);
      }
     console.log('here comes',this.tempVar)
     console.log('here comes temp', this.tempVarDt);
    }

  },
 
})
</script>

<style>



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
  
#sidebarmain {
  position: relative;
  display: flex;
  flex-direction: column;
  background-image: url('assets/morning.jpg');
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
  font-size: 6rem;
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
  font-size: 1.25rem;
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
  right: 0;margin-top: 4px;
  top: 16px;
  padding: 2px;
  z-index: 999;
  height: 42px;
  width: 45px;
  border: none;
  font-size: 38px
}

#dashboard-content {
  text-align: center;
  height: 100vh;
}

#date-desc,
#location-desc {
  color: #ffffff;
  font-size: 1.2rem;
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
  margin-left:30px;
  margin-top:30px
}

@media only screen and (min-width: 768px) {
  #sidebarmain {
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
  #sidebarmain {
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