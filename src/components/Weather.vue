<template>
  <div class="hello">
    <div>
<input type="text" placeholder="Search..." class="search-bar" 
v-model="query" v-on:keypress="fetchWeather" />
</div>
<div class="date"><p>
  {{ todaysDate() }}</p></div>

<transition name="fade">
<div class="weather-box" v-if="weatherBox">
<h1 v-if="weather.sys">
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
  };
 },
computed: {

},
 methods: {
   async fetchWeather(e) {
 if (e.key == "Enter") {
  let response = await axios.get(`${this.url_base}weather?q=${this.query}&appid=${this.api_key}`);
  this.weatherBox = true;
 this.weather = response.data;
 this.query = "";

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
