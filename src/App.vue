<template>
  <div id="app" :class="typeof weather.main!='undefined' && weather.main.temp>28?'warm' : ' ' ">
    <main>
       <v-responsive :aspect-ratio="16/9">
         <div class="search-box">
        <input 
        type="text" class="search-bar" placeholder="Search.." v-model="query" @keypress="fetchWeather"/>
        <!-- {{query}} -->
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined' ">
        <div class="location-box">
          <div class="location">{{weather.name}} / {{weather.sys.country}}</div>
          <div class="weather">{{weather.weather[0].main}}/{{weather.weather[0].description}}</div>
          <div class="date">{{dateBuilder()}}</div>
        </div>
        <div class="weather-box">
          <h1>Now</h1>
          <div class="temp"> {{Math.round(weather.main.temp)}}°C</div>
          
        </div>
        
        <div class="all-details">
          <div class="dimension">
            <p>Longitude: {{weather.coord.lon}}</p>
            <p>Latitude:{{weather.coord.lat}}</p>
          </div>
          <br/>
          <div class="about-temp">
            <div class="feels-like">Feels like: 
              {{weather.main.feels_like}}°C</div>
            <div class="temp_min">Forecast: {{weather.main.temp_min}}°C / {{weather.main.temp_max}}°C</div>
             <br/>
            <div class="pressure">Pressure: {{weather.main.pressure}}</div>
            <div class="humidity">Humidity: {{weather.main.humidity}}</div>
             <br/>
          </div>
          <div class="timezone">Timezone: {{weather.timezone/3600}} </div>
        </div>
      </div>
      </v-responsive>
    </main>
  </div>
</template>

<script>

export default {
  name: 'App',
  data(){
    return {
      api_key: '866b147e923716bf116b2b81e4cd3ca4',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather(e){
      if(e.key=="Enter"){
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
        .then(res => {
          console.log(res.json);
          return res.json();
          
        }).then(this.setResults);
      }
    },
    setResults(results){
      this.weather=results;
    },
    dateBuilder(){
      let d=new Date();
      let months=["January","February","March","April","May","June","Jul","August","September","October","November","December"];
      let days=["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"]
      let day=days[d.getDay()];
      let date=d.getDate();
      let month=months[d.getMonth()];
      let year=d.getFullYear();
      return `${day}, ${date} ${month} ${year}`
    }
  }
}
</script>

<style>
/* 
#app
  main
  .search-box
    input.search-bar
  .weather-wrap
    .location-box
      .location
      .date
    .weather-box
      .temp
      .weather 
    .all-details
      .dimension
        .long 
        .lat
      .about-temp
        .feels-like
        .temp_min
        .temp_max
        .pressure
        .humidity
        .timezone
*/
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  color: #fff;
}
body{
  font-family: 'montserrat',sans-serif;
}
#app{
  background-image: url('./assets/cold-bg.jpg');
  background-size: cover;
  background-position: bottom;
  transition: .4s;
}
#app.warm{
  background-image: url('./assets/warm-bg.jpg');
}
main{
  min-height: 100vh;
  padding: 2rem;
  background-image: linear-gradient(to bottom,rgba(0,0,0,0.25),rgba(0,0,0,0.75));
}
.search-box{
  width: 100%;
  margin-bottom: 1rem;
}
.search-box .search-bar{
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131; 
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  background-color: rgba(255,255,255,0.5);
  box-shadow: 0px 0px .5rem rgba(0,0,0,0.25);
  border-radius: 0px 1rem;
  transition: 0.4s;
}
.search-box .search-bar:focus{
  box-shadow: 0px 0px 1rem rgba(0,0,0,0.25);
  background-color: rgba(255,255,255,0.75);
  border-radius: 1rem 0;
}
.location-box{
  margin: 0.5rem;
}
.location-box .location{
  font-size: 1.5rem;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0,0,0,0.25);
}
.location-box .weather{
  font-size: 1.2rem;
  font-weight: 700;
  text-align: center;
  font-style: italic;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
  margin: 0.1rem;
}
.location-box .date{
  font-size: 0.8rem;
  font-weight: 300;
  text-align: center;
  font-style: italic;
}
.weather-box{
  text-align: center;
  display: flex;
  justify-content: center;
  /* flex-direction: column; */
}
.weather-box h1{
  margin: 0.5rem;

}
.weather-box .temp{
  display: inline-block;
  padding: 10px 25px;
  font-size: 2rem;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255,0.25);
  margin: 0 0 1rem;
  border-radius: 16px;
  box-shadow: 3px 6px 3px 3px rgba(0,0,0,0.25);
}
.weather-box .weather{
  font-size: 1.2rem;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
  margin: 0.5rem;
} 
.all-details{
  display: flex;
  /* border: 1px solid #fff; */
  box-shadow: 4px 4px.4rem .4rem rgba(0,0,0,0.55);
  border-radius: 1rem;
  padding: 1.5rem;
  flex-direction: column;
  /* width: auto; */
  width: 33%;
  margin: 1rem auto;
  /* text-align: center;  */
  font-size: 1rem;
}

/* @media screen and (max-width: 768px) {
  .all-details{
    width: 50%;
  }
} */
@media screen and (min-width: 475px) and (max-width: 768px){
  .all-details{
    width: 50%;
  }
}
@media screen and (min-width: 320px) and (max-width: 475px){
  .all-details{
    width: auto;
  }
}
</style>
