<template>
  <div
    id="app"
    :class="
      typeof weather.main != undefined && weather.main.temp > 16 ? `warm` : ``
    "
  >
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="lat..."
          v-model="lat"
          @keypress="fetchWeather"
        />
      </div>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="long..."
          v-model="long"
          @keypress="fetchWeather"
        />
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">
            {{ weather.name }},{{ weather.sys.country }}
          </div>
          <div class="date">Monday 20 January 2022</div>
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
  props: [],
  data() {
    return {
      //open-weather-api-key
      api_key: "5993ab2fa550917ace8aaabd8acbf522",
      url_base: "https://api.openweathermap.org/data/2.5/",
      lat: 0,
      long: 0,
      weather: {},
    };
  },
  methods: {
    async fetchWeather(e) {
      //e = event
      if (e.key == "Enter") {
        await fetch(
          `${this.url_base}weather?lat=${this.lat}&lon=${this.long}&units=metric&appid=${this.api_key}`
        )
          .then((res) => {
            return res.json();
          })
          .then(this.setResults);
      }
    },
    setResults(data) {
      console.log(data);
      this.weather = data;
    },
    dateBuilder() {},
  },
  components: {},
  watch: {},
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
  background-image: url("./assets/cold-bg.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app .warm {
  background-image: url("./assets/warm-bg.jpg");
}

main {
  min-height: 100vh;
  padding: 25px;

  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
}

.search-box .search-bar {
  display: block;
  width: 100%;

  padding: 15px;

  color: rgb(31, 31, 31);
  font-size: 20px;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  background-color: rgb(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgb(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
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

.weather {
  color: white;
  font-weight: 300;
  font-size: 50px;
  font-style: italic;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: white;
  font-size: 102px;
  font-weight: 600;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(255, 255, 255, 0.25);
}
</style>
