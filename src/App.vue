<template>
  <div
    id="app"
    class="app"
    :class="
      typeof weather.main != 'undefined' && weather.main.temp > 18 ? 'warm' : ''
    "
  >
    <main>
      <div class="search-box">
        <input
          v-model="query"
          type="text"
          class="search-bar"
          placeholder="Search.."
          @keypress.enter="fetchData()"
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
          <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
          <div class="weather">
            <i>{{ weather.weather[0].main }}</i>
          </div>
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
      api_key: "6f36fa705fb37a3c0bf995fe5c95b68e",
      url_base: "http://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
    };
  },
  methods: {
    fetchData() {
      fetch(
        `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
      )
        .then((res) => {
          return res.json();
        })
        .then(this.setWeather);
    },

    setWeather(results) {
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
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
.app {
  width: 100%;
  height: 100vh;
  background-image: url("./assets/img/1.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
.app.warm {
  background-image: url("./assets/img/2.jpg");
}
.app main {
  min-height: 100vh;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.2),
    rgba(0, 0, 0, 0.4)
  );
}
.app main .search-box {
  padding: 0 20px;
  display: flex;
  justify-content: center;
  margin-bottom: 30px;
}
.app main .search-box input::placeholder {
  color: black !important;
}
.app main .search-box input {
  width: 70%;
  margin: 0 auto;
  padding: 30px 20px;
  border-radius: 0 40px 0 40px;
  background-color: rgba(255, 255, 255, 0.6);
  border: none !important;
  font-size: 20px;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
}
.app main .search-box input:focus {
  border: none !important;
  background-color: rgba(255, 255, 255, 0.75);
  outline: 0 !important;
  transition: 0.5s;
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.5);
  border-radius: 40px 0 40px 0;
}
.location-box .location {
  color: #fff;
  font-size: 42px;
  text-align: center;
  font-weight: 500;
  text-shadow: 3px 3px rgba(50, 50, 70, 0.5);
}
.location-box .date {
  color: #fff;
  font-size: 25px;
  font-weight: 300;
  text-align: center;
  font-style: italic;
  text-shadow: 2px 2px rgba(50, 50, 70, 0.5);
}
.weather-box {
  text-align: center;
}
.weather-box .temp {
  position: relative;
  display: inline-block;
  margin: 30px auto;
  background-color: rgba(255, 255, 255, 0.2);
  border-radius: 15px;
  padding: 15px 25px;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(50, 50, 70, 0.5);
  color: #fff;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.2);
}

.weather-box .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  text-shadow: 3px 6px rgba(50, 50, 70, 0.5);
}
</style>
