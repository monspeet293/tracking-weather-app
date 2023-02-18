<template>
  <div id="app">
    <main>
      <div class="search-box">
        <input type="text" class="search-bar" placeholder="Search..." v-model="query" @keypress="fetchWeather" />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ setDate() }}</div>
        </div>

        <div class="weather-box">
          <div class="temperature">{{ Math.round((weather.main.temp) / 10) }}°C</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
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
      api_key: '1cd13fe5f5e46061fec18fbcafce5c3f',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}

    }
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        const place = (this.query.trim()).replace("  ", "");
        console.log(place);
        fetch(`${this.url_base}weather?q=${(place)}&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
    },
    setDate() {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

      return `${days[d.getDay()]} ${d.getDate()} ${months[d.getMonth()]} ${d.getFullYear()}`
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'montserral', sans-serif;
}

#app {
  background-image: url('./assets/background.png');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

main {
  min-height: 100vh;
  padding: 25px;

  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.1), rgba(0, 0, 0, 0.6));
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

  box-shadow: 0px 0px 8px rgba(152, 172, 255, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 20px;
  transition: 0.3s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 8px rgba(152, 172, 255, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 10px;
}

.location-box .location {
  color: #FFF;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.6);
}

.location-box .date {
  color: #FFF;
  font-size: 20px;
  font-weight: 100;
  font-style: italic;
  text-align: center;
  /* text-shadow: 1px 3px rgba(0, 0, 0, 0.6); */
}

.weather-box {
  text-align: center;
  color: #FFF;
}

.weather-box .temperature {
  display: inline-block;
  padding: 10px 25px;
  font-size: 100px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.5);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 20px;
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.5);

}

.weather-box .weather {
  font-size: 48px;
  font-weight: 700;
  font-style: italic;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.5);

}
</style>
