<template>
  <div class="hello">
    
    <transition name="bounce">
    <div v-if="searchBox" class="search-box">
    
<input type="text" placeholder="Search..." class="search-bar" 
v-model="query" v-on:keypress="fetchWeather" />
</div>
    </transition>
<!--<div class="date"><p>
  {{ todaysDate() }}</p></div> -->
<transition name="fade">
<div class="flex-weather-box" v-if="weather.weather" :style="{background: project_status}">
  
  <i class="fas fa-chevron-down arrow" @click="searchBox = !searchBox"></i>


<div class="flex-weather" :style="{boxShadow: project_shadow}">
<div class="weather-box" v-if="weatherBox" >
<h1 v-if="weather.sys" >
  {{weather.name}}, {{weather.sys.country}}
</h1>
<p class="description">{{ weather.weather[0].description }}</p>

<h2 > {{ Math.round((weather.main.temp - 273.15))}}°c</h2>
<div v-if="weather.main" class="flex-temp">

<p> Min: {{ Math.round((weather.main.temp_min - 273.15))}}°c</p>
<p> Max: {{ Math.round((weather.main.temp_max- 273.15))}}°c</p>
</div>
  <div class="weather" v-if="weather.weather">
 
  <img v-bind:src="'http://openweathermap.org/img/wn/' + weather.weather[0].icon + '@2x' + '.png' "  /> 

 </div>
</div>



<div class="additional-weather-box" v-if="weatherBox">
  <p><i class="fas fa-tint"></i>   Humidity: {{weather.main.humidity}}%</p>
  <p><i class="fas fa-wind"></i> Wind speed: {{weather.wind.speed}} km/h</p>
  <p><i class="fas fa-compress-arrows-alt"></i> Pressure: {{weather.main.pressure}} hPa</p>
  <p><i class="fas fa-eye"></i> Visibilty: {{weather.visibility}} </p>
  <p><i class="fas fa-cloud"></i>Clouds: {{weather.clouds.all}} %</p>
  <p><i class="fas fa-temperature-low"></i>Feels like: {{ Math.round((weather.main.feels_like - 273.15))}}°c</p>
  </div>

</div>
</div>
</transition>
  </div>

</template>

<script>
import axios from 'axios';

export default {
  name: "Weather",
 data() {
  return {
   api_key: "bf038f1354be81698da7c76577ad06c6",
   url_base: "https://api.openweathermap.org/data/2.5/",
   weather_icon: "http://openweathermap.org/img/wn/",
   query: "",
   weather: {},
   weatherBox:false,
   searchBox:true,
   colorBack: "linear-gradient(164deg, rgba(211,211,212,1) 3%, rgba(175,176,176,1) 26%, rgba(150,153,154,1) 56%)", 
   colorBackClear: "linear-gradient(164deg, rgba(190,214,221,1) 3%, rgba(147,200,213,1) 26%, rgba(116,171,185,1) 56%)",
   colorShadow: "5px 10px 18px rgb(75, 84, 87)", 
   colorShadowClear: "5px 10px 18px rgb(78, 123, 136)"
  };
 },
computed: {
    project_status() {
        if (this.weather.weather[0].main === 'Clear') return this.colorBackClear;
        else if (this.weather.weather[0].main === 'Clouds') return this.colorBack;
        
       return "red"
    }, 

       project_shadow() {
        if (this.weather.weather[0].main === 'Clear') return this.colorShadowClear;
        else if (this.weather.weather[0].main === 'Clouds') return this.colorShadow;
        
       return "red"
    }
},

  watch: {
  
  }, 

 methods: {

searchOpen() {
this.searchBox = true;
},



   isRed() {
      document.querySelector('body').style.backgroundColor = (this.weather.weather[0].icon === '01n' ) ? 'red' : null;
      console.log("hello")
      console.log(this.colorBack)
    },

   async fetchWeather(e) {
 if (e.key == "Enter") {
  let response = await axios.get(`${this.url_base}weather?q=${this.query}&appid=${this.api_key}`);
  this.weatherBox = true;
 this.weather = response.data;
 this.query = "";
this.searchBox = false;
 console.log(this.weather)
 }
},


todaysDate() {
const months = [
"Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sep","Oct","Nov",
"Dec",];
const days = ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"];
let d = new Date();
let month = months[d.getMonth()];
let day = days[d.getDay()];
let date = d.getDate();
let year = d.getFullYear();
return `${month} ${date} ${day} ${year}`;
},

 }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

@import "@/assets/styles/weather.scss";

h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
