<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
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
          <div class="location">{{ weather.name }}, {{ weather.sys.country}}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temperature">{{ Math.round(weather.main.temp) }}°C</div>
          <div class="weather-status">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      api_key: '6bbff6f0aab27e3a6c65d698546bbe76',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
        .then(res => {
          return res.json();
        }).then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
    },
    dateBuilder() {
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"];
      let d = new Date();

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    }
  }
}
</script>

<style>
@-ms-viewport{
  width: device-width;
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Montserrat', sans-serif;
}

#app {
  background-image: url('./assets/cold-bg.jpg');
  background-size: cover;
  background-position: top;
  transition: 0.4s;
}

#app.warm {
  background-image: url('./assets/warm-bg.jpg');
}

main {
  min-height: 100vh;
  padding: 25px;

  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.2), rgba(0, 0, 0, 0.6));
}

.search-box {
  width: 100%;
  margin-bottom: 30px;

}

@keyframes slideInTopToBottom{
  0% {
    transform: translateY(-100%);
  }
  100% {
    transform: translateY(0%);
  }
}

.search-box .search-bar {
  animation: 1s slideInTopToBottom;
  min-width: 100%;
  padding: 15px;

  color: #313131;
  font-size: 20px;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.3);
  border-radius: 2px 25px 2px 25px;
  transition: 0.3s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.60);
  border-radius: 25px 2px 25px 2px;
}

.location-box .location {
  color: white;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date {
  color: white;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.weather-box {
  text-align: center;
}

.weather-box .temperature {
  display: inline-block;
  padding: 10px 25px;
  color: white;
  font-size: 100px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 25px;
  margin: 30px;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather-status {
  color: white;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 5px rgba(0, 0, 0, 0.25);
}

</style>