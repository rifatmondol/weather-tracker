<template>
  <div id="app">
    <main>
      <div class="search-section">
        <input
          type="text"
          class="search-box"
          placeholder="Search"
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">
            {{ weather.name }}, {{ weather.sys.country }}
          </div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ Math.ceil(weather.main.temp) }}°c</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      api_key: "f0685f88f0aa91761a1540c360791255",
      url_base: "http://api.openweathermap.org/data/2.5/",
      query: "",
      weather: "",
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
        )
          .then((res) => {
            return res.json();
          })
          .then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
    },
    dateBuilder() {
      let d = new Date();
      let months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      let days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Montserrat:wght@300;700;900&display=swap");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: "Montserrat", sans-serif;
}
#app {
  background-image: url(./assets/cold-weather.jpg);
  background-position: bottom;
  background-size: cover;
  max-width: 768px;
  margin: 0 auto;
  transition: 0.4s;
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
.search-section {
  margin-bottom: 40px;
}
.search-box {
  display: block;
  padding: 15px;
  width: 100%;
  transition: 0.4s;
  font-size: 20px;
  appearance: none;
  border: none;
  background: none;
  outline: none;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  border-radius: 0px 16px 0px 16px;
  background: rgba(255, 255, 255, 0.25);
}
.search-box:focus {
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  border-radius: 16px 0px 16px 0px;
  background: rgba(255, 255, 255, 0.75);
}
.location-box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 700;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  text-align: center;
}
.location-box .date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}
.weather-box {
  text-align: center;
}

.weather-box .temp {
  font-size: 100px;
  color: #fff;
  display: inline-block;
  padding: 15px 25px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather-box .weather {
  font-size: 48px;
  color: #fff;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  font-weight: 700;
  font-style: italic;
}
</style>
