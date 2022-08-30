<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 20 ?
    'warm' : '' ">
    <main>
        <div class="search-box">
          <input
            type="text"
            class="search-bar"
            placeholder="Search..."
            v-model="query"
            @keypress="fetchWeather"
          />
        </div>
        <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
          <div class="location-box">
            <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
            <div class="date">{{ todaysDate() }}</div>
          </div>
          <div class="weather-box">
            <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
            <div class="weather">{{ weather.weather[0].description }}</div>
          </div>
        </div>
    </main>
  </div>
</template>

<script>

export default {
  name: 'App',
  data () {
    return{
      api_key: 'ad9f08b0d7f65806f6450e75e3838d0d',
      base_url: 'http://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather (e) {
      if(e.key == "Enter"){
        fetch(`${this.base_url}weather?q=${this.query}&units=metric&appid=${this.api_key}&lang=pt_br`)
        .then(res => {
          return res.json();
        }).then(this.setResults);
      }
    },
    setResults ( results ){
      this.weather = results;
    },
    todaysDate() {
      const months = [
      "Janeiro","Feveiro","Mar","Abril","Maio","Junho","Julho","Agosto","Setembro","Outubro","Novembro","Dezembro"];
      const days = ["Dom", "Seg", "Ter", "Qua", "Qui", "Sex", "Sab"];
      let d = new Date();
      let month = months[d.getMonth()];
      let day = days[d.getDay()];
      let date = d.getDate();
      let year = d.getFullYear();
      return `${day}, ${date} de ${month} ${year}`;
    },
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body{
  font-family: 'montserrat', sans-serif;
}
#app{
  background: #87ceeb;
  background-position: bottom;
  background-size: cover;
  transition: 0.4s;
}
#app.warm{
  background: #F1CD6C;
}
main{
  min-height: 100vh;
  padding: 2rem;

  background-image: linear-gradient(to bottom, rgba(0,0,0,0.25), rgba(0,0,0,0.75));
}
.search-box {
  width: 100%;
  margin-bottom: 3rem;
}
.search-box .search-bar{
  display: block;
  width: 100%;
  padding: 1.2rem;
  color: #313131;

  font-size: 20px;

  appearance: none;
  border: none;
  outline: none;

  background: none;
  box-shadow: 0px 0px 8px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255,0.5);
  border-radius: 0px 16px 0px 16px;
  transition: .4s;
}
.search-box .search-bar:focus{
  box-shadow: 0px 0px 16px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255,0.75);
  border-radius: 16px 0px 16px 0px;

}
.location-box .location{
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0,0,0,0.25);
}
.location-box .date{
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  text-align: center;
  font-style: italic;
}

.weather-box{
  text-align: center;
}

.weather-box .temp{
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255,0.25);
  border-radius: 16px;
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(0,0,0,0.25);
}
.weather-box .weather{
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
}
</style>
