<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp < 0 ? 'cold' : '' && weather.main.temp > 20 ? 'warm' : '' && weather.main.temp > 30 ? 'sun' : ''">    
  <main>
      <div class="search-box">
        <input 
        type="text" 
        class="search-bar" 
        placeholder="Search..." 
        v-model="query"
        @keypress="showWeath"
        />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ datebuild() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>
<script>
export default {
  name: "app",
  data() {
    return {
      api_key: "83f254ec0e87ff55874b4d8eb3c67809",
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {},
    };
  },
  methods: {
    showWeath(e){
      if (e.key == 'Enter') {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
        .then(res => {
          return res.json()
        }).then(this.setResults);
      }
    },
    setResults(results){
      this.weather = results
    },
    datebuild(){
      let d = new Date()
      let months = ['January', 'February', 'March', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December']
      let days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday']

      let day = days[d.getDay()]
      let date = d.getDate()
      let month = months[d.getMonth()]
      let year = d.getFullYear()

      return `${day} ${date} ${month} ${year}`;
    }
  },
};
</script>
<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: "montserrat", sans-serif;
}
#app {
  background-image: url("https://wallpapercave.com/wp/wp3584348.jpg");
  background-size: cover;
  background-position: center;
  transition: 4s;
}
#app.warm{
  background-image: url("https://images.alphacoders.com/938/938875.jpg");
}
#app.cold{
  background-image: url("https://wallpapercave.com/wp/wp3335309.jpg");
}
#app.sun{
  background-image: url("https://wallpapercave.com/wp/wp2268961.jpg");
}
main {
  display: grid;
  grid-template-columns: 50% 50%;
  min-height: 100vh;
  padding: 30px;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0,.25), rgba(0, 0, 0, .6));
}
.search-box .search-bar{
  display: block;
  margin-top: 30vh;
  width: 100%;
  padding: 15px;
  color: #000000;
  font-size: 20px;
  border: none;
  background: none;
  outline: none;
  appearance: none;
  background-color: rgba(255, 255, 255, .5);
  border-radius:  20px;
  transition: .4s;
}
.search-box .search-bar:focus{
  box-shadow: 0px 0px 16px rgba(0, 0, 0, .25);
  background-color: rgba(255, 255, 255, .75);
}
.location-box .location{
  color: #fff;
  margin-top: 10vh;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
}
.location-box .date{
  color: #fff;
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
  color: #fff;
  font-size: 100px;
  font-weight: 900;
  text-shadow: rgba(0, 0, 0, .25);
  border-radius: 20px;
  margin: 30px 0;

}
.weather-box .weather{
  color: white;
  font-size: 20px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, .25);
}
@media (max-width: 400px) {
  main{
    display: flex;
    flex-direction: column;
  }
  .search-box .search-bar{
    margin-top: 5vh;
  }
}
</style>
