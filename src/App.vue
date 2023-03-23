<template>
  <div id="app" :class="weather.name != null && Math.round(weather.main.temp - 273.15) < 16 ? 'cold' : ''">
    <b-container>
      <div class="app__inner">
        <b-form class="app__form" v-on:submit.prevent="fetchWeather">
          <b-form-input v-model="query" placeholder="Search..."></b-form-input>
        </b-form>
        <div class="app__data-block" v-if="weather.name != null">
          <div class="app__meta">
            <div class="app__location">
              {{ weather.name }}, {{ weather.sys.country }}
            </div>
            <div class="app__date">
              {{ dateBuilder() }}
            </div>

          </div>
          <div class="app__weather">
            <div class="app__temp">{{ Math.round(weather.main.temp - 273.15) }}°C</div>
            <div class="app__precip">{{ weather.weather[0].main }} </div>
          </div>
        </div>
      </div>
    </b-container>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      api_key: '4bb9e217581f5c2dcea71bc00aaceaa0',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather() {
      fetch(`${this.url_base}weather?q=${this.query}&APPID=${this.api_key}`)
        .then(res => {
          console.log(res)
          return res.json();

        }).then(this.setResults);
    },
    setResults(res) {
      this.weather = res;
    },
    dateBuilder() {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
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
@import url('https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@0,300;0,500;0,600;0,700;1,400&display=swap');

*,
*:after,
*:before {
  box-sizing: border-box;
  font-family: 'Montserrat', sans-serif;
}

html,
body {
  margin: 0;
  padding: 0;
  font-family: 'Montserrat', sans-serif;
  color: white !important;
}

.app__inner {
  background-image: url(./assets/sunny.jpg);
  background-position: center;
  background-size: cover;
  padding: 60px 20px 300px 20px;
  position: relative;
  border-radius: 20px;
  overflow: hidden;
}

.cold .app__inner {
  background-image: url(./assets/rainy.jpg);
}

.app__inner:before {
  position: absolute;
  content: '';
  background: rgba(0, 0, 0, 0.4);
  backdrop-filter: blur(1px);
  width: 100%;
  height: 100%;
  left: 0;
  top: 0;
}

.form-control {
  border-radius: 0 8px 0 8px !important;
}

.app__form {
  position: relative;
  z-index: 2;
}

.app__data-block {
  position: relative;
  z-index: 2;
}

.app__location {
  text-align: center;
  font-size: 40px;
  margin-top: 30px;
}

.app__date {
  text-align: center;
  font-size: 16px;
  opacity: 0.7;
  margin-bottom: 30px;
  letter-spacing: 2px;
}

.app__temp {
  text-align: center;
  padding: 6px 28px;
  border-radius: 12px;
  background: rgba(255, 255, 255, 0.378);
  box-shadow: 3px 3px 8px rgba(0, 0, 0, 0.39);
  margin: auto;
  margin-bottom: 20px;
  font-size: 70px;
  width: fit-content;
  font-weight: 700;
  backdrop-filter: blur(1.4px);
}

.app__precip {
  text-align: center;
  font-size: 30px;
  letter-spacing: 2px;
  font-style: italic;
}

@media(min-width: 600px) {
  .container {
    max-width: 500px !important;
  }
}
</style>
