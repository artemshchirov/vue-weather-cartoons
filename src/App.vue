<template>
  <div
    id="app"
    :class="typeof weather.main !== `undefined` && setBackground()"
    v-cloak
  >
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
      <div class="weather-wrap" v-if="typeof weather.main !== `undefined`">
        <div class="location-box">
          <p class="location">{{ weather.name }}, {{ weather.sys.country }}</p>
          <p class="date">{{ dateBuilder() }}</p>
        </div>
        <div class="weather-box">
          <p class="temp">{{ Math.round(weather.main.temp) }}°C</p>
          <p class="weather">{{ weather.weather[0].main }}</p>
        </div>
        <p class="cartoon">{{ setCartoon() }}</p>
        <user-location></user-location>
      </div>
    </main>
  </div>
</template>

<script>
import UserLocation from './components/UserLocation.vue';

export default {
  el: '#app',
  components: {
    UserLocation,
  },
  data() {
    return {
      api_key: 'd1cbf50ad630c263341b5810669be5a1',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {},
      selectedIndex: -1,
    };
  },
  created() {
    this.fetchWeatherDefault();
    this.setCartoon();
  },

  methods: {
    fetchWeather(e) {
      if (e.key === 'Enter') {
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
        )
          .then((res) => res.json())
          .then((res) => {
            this.setResults(res);
            this.setCartoon();
          });
      }
    },

    fetchWeatherDefault(city = 'Haifa') {
      fetch(
        `${this.url_base}weather?q=${city}&units=metric&APPID=${this.api_key}`
      )
        .then((res) => res.json())
        .then(this.setResults);
    },

    setResults(results) {
      this.temp = Math.round(results.main.temp);
      this.weather = results;
    },

    setCartoon() {
      switch (true) {
        case this.temp > 35:
          return 'BoJack Horseman';
        case this.temp > 15:
          return 'Final Space';
        case this.temp > 0:
          return 'Solar Opposites';
        case this.temp < 0:
          return 'Love Death + Robots';
        default:
          return 'Ed, Edd n Eddy';
      }
    },

    setBackground() {
      switch (true) {
        case this.temp > 35:
          return 'bgc-4';
        case this.temp > 15:
          return 'bgc-3';
        case this.temp > 0:
          return 'bgc-2';
        case this.temp < 0:
          return 'bgc-1';
        default:
          return 'bgc-0';
      }
    },

    dateBuilder() {
      let d = new Date();

      let days = [
        'Sunday',
        'Monday',
        'Tuesday',
        'Wednesday',
        'Thursday',
        'Friday',
        'Saturday',
      ];
      let months = [
        'January',
        'February',
        'March',
        'April',
        'May',
        'June',
        'July',
        'August',
        'September',
        'October',
        'November',
        'December',
      ];

      let date = d.getDate();
      let day = days[d.getDay()];
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    },
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
  font-family: 'montserrat', sans-serif;
}

#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  background-color: black;
  background-size: cover;
  background-repeat: no-repeat;
  background-position: top;
  transition: 0.4s;
  position: relative;
}

#app.bgc-0 {
  background-image: url('./assets/bgc-0.jpg');
}

#app.bgc-1 {
  background-image: url('./assets/bgc-1.jpg');
}

#app.bgc-2 {
  background-image: url('./assets/bgc-2.jpg');
}

#app.bgc-3 {
  background-image: url('./assets/bgc-3.jpg');
}

#app.bgc-4 {
  background-image: url('./assets/bgc-4.jpg');
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

  color: #313131;
  font-size: 20px;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0 16px 0 16px;
  transition: 0.4s;
}

.search-box .search-bar::placeholder {
  color: whitesmoke;
  opacity: 1;
}

.search-box .search-bar:focus {
  box-shadow: 0 0 16px rgba(255, 255, 255, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0 16px 0;
}

.weather-wrap {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  margin: auto;
  padding: 25px;
  max-width: max-content;
  max-height: max-content;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  backdrop-filter: blur(5px);

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.location-box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
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

.temp {
  margin: 25px 0;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 64px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather {
  color: #fff;
  font-size: 32px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.cartoon {
  color: #fff;
  font-size: 50px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);

  margin: 25px 0;

  text-transform: uppercase;
  background: linear-gradient(to right, white 0%, pink 100%);
  background-clip: text;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}
</style>
