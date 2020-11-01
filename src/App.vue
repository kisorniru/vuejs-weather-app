<template>
  <div id="app" :class="typeof weather.main != 'undefined' ? weatherCondition : ''">
    <main>
      <div class="search-box">
        <input 
        type="text"
        class="search-bar"
        placeholder="Search..."
        v-model="query"
        @keypress="fetchWeather"
        >
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">Sunday 02 November 2020</div>
        </div>

        <div class="weather-box">
          <div class="temp">
            <p>{{ Math.round(weather.main.temp) }}°C</p>
            <span>Feels like {{ Math.round(weather.main.feels_like) }}°C</span>
          </div>
          <div class="weather">
            <img v-bind:src="weatherIcon" />
            <br>
            {{ weather.weather[0].main }}</div>
        </div>
      </div>

    </main>
  </div>
</template>

<script>

export default {
  data() {
    return {
      api_key: '98011d50c69ba158575cee8a1b7a9aa3',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weatherIcon: '',
      weatherCondition: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather (e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`).then(response => {
          return response.json();
        }).then(this.setResults);
      }
    },
    setResults (results) {
      this.weatherIcon = 'http://openweathermap.org/img/w/'+results.weather[0].icon+'.png';
      this.weatherCondition = results.weather[0].main.toLowerCase();
      this.weather = results;
    }
  }
}
</script>

<style>
/*  */
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  body {
    font-family: 'montserrat', sans-serif;
  }

  #app {
    background-image: url('./assets/clear-bg.jpg');
    background-size: cover;
    background-position: bottom;
    transition: 0.4s;
  }

  #app.thunderstorm {
    background-image: url('./assets/thunderstorm-bg.jpg');
  }

  #app.drizzle {
    background-image: url('./assets/drizzle-bg.jpg');
  }

  #app.rain {
    background-image: url('./assets/rain-bg.jpg');
  }

  #app.snow {
    background-image: url('./assets/cold-bg.jpg');
  }

  #app.clouds {
    background-image: url('./assets/warm-bg.jpg');
  }

  main {
    min-height: 100vh;
    padding: 25px;
    background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
  }

  .search-box {
    width: 100%;
    margin-bottom: 30px;
  }

  .search-box .search-bar {
    display: block;
    width: 100%;
    padding: 15px;

    color: #313131;
    font-size: 20px;

    appearance: none;
    border: none;
    outline: none;
    background: none;

    box-shadow:  0px 0px 8px 0px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.5);
    border-radius: 0px 16px 0px 16px;
    transition: 0.4s;
  }

  .search-box .search-bar:focus {
    box-shadow:  0px 0px 16px 0px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.75);
    border-radius: 16px 0px 16px 0px;
  }

  .location-box {
    color: #FFF;
    text-align: center;
  }

  .location-box .location {
    font-size: 32px;
    font-weight: 500;
    text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  }

  .location-box .date {
    font-size: 20px;
    font-weight: 300;
  }

  .weather-box {
    color: #FFF;
    text-align: center;
  }

  .weather-box .temp {
    display: inline-block;
    padding: 10px 25px;
    
    background-color: rgba(255, 255, 255, 0.25);
    border-radius: 16px;
    margin: 30px 0px;

    box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  }

  .weather-box .temp p {
    font-size: 102px;
    font-weight: 900;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  }

  .weather-box .temp span {
    font-size: 20px;
    font-weight: 100;
  }

  .weather-box .weather {
    font-size: 48px;
    font-weight: 700;
    font-style: italic;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  }

</style>
