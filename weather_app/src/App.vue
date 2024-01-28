<script>
export default {
  name:'App',
  data(){
    return {
      api_key:'006e2bebf795c269b2e0ff117ec47592',
      url_base:'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods:{
    fetch_weather(e){
      if(e.key==="Enter"){
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
        .then(res => {
          return res.json();
        }).then(this.setResults);
      }
    },
    setResults(results){
      this.weather=results;
    },
    dateBuilder(){
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    }
  }
}
</script>

<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'hot' : ''">
    <main>
      <div class="search-box">
        <input 
          type="text" 
          class="search-bar" 
          v-model="query"
          @keypress="fetch_weather"
        />
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
          <div class="weather-image">
            <img id="wicon" alt="Weather Icon" :src="`https://openweathermap.org/img/wn/${weather.weather[0].icon}.png`" />
          </div>
          <div class="weather">
            {{ weather.weather[0].main }}
          </div>
          <div class="weather-description">
            {{ weather.weather[0].description }}
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<style scoped>
*{
margin:0;
padding: 0;
box-sizing: border-box;
}
body {
  font-family: 'montserrat',sans-serif;
}
#app{
  background-image: url("assets/cold-bg.png");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
#app .hot{
  background-image: url(assets/hot-bg.png);
}
main{
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom,rgba(0,0,0,0.25),rgba(0,0,0,0.25));
}
.search-box{
  width:100%;
  margin-bottom: 30px;
}
.search-box .search-bar {
display: block;
width: 100%;
padding:15px;
color:#313131;
font-size: 20px;
box-shadow: 0px 0px 16px rgba(0,0,0,0.25);

appearance: none;
border: none;
background: none;
outline: none;

background-color: rgba(255,255,255,0.5);
border-radius: 10px 10px 10px 10px;
transition: 0.4s;
}
.search-box .search-bar :focus {
background-color: rgba(255,255,255,0.75);
box-sizing: 0px 0px 16px rgba(0,0,0,0.25);
border-radius: 6px 0px 16px 0px;
}
.location-box .location{
  color:#FFFFFF;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0,0,0,0.25);
}
.location-box .date {
color:#FFFFFF;
font-size: 20px;
font-weight: 300;
font-style: italic;
text-align: center;
}
.weather-box .date {
  color:#FFFFFF;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;

}
.weather-box{
  text-align: center;
}
.weather-box .temp{
display: inline-block;
padding: 10px 25px;
color: #FFFFFF;
font-size: 102px;
font-weight: 900;

text-shadow: 3px 6px rgba(0,0,0,0.25);
background-color: rgba(255,255,255,0.25);
border-radius: 16px;
margin:30px 0px;

box-shadow: 3px 6px rgba(0, 0,0,0.25);
}
.weather-box .weather-description {
  color:#FFFFFF;
  font-size: 10px;
  font-weight: 300;
  font-style: italic;
}
.weather-box .weather-image{
  position: center;
}

@media (min-width: 1024px) {
  .search-box {
    width: 50%;
    margin: 0 auto;
  }
  .location-box .location, .location-box .date {
    font-size: 24px; 
  }
  .weather-box .temp {
    font-size: 80px; 
  }
  .weather-box .weather-description {
    font-size: 14px;
  }
}

/* Styles réactifs pour les écrans plus petits (exemple : tablettes) */
@media (max-width: 768px) {
  
}
/* Styles réactifs pour les écrans plus petits téléphone */
@media (max-width: 480px) {
  
  }
</style>
 