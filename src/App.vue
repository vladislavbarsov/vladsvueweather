<template>
  <div id="app" :class="typeof weather.city != 'undefined' && forecastdaysmaxdegs[0] > 16 ? 'warm' : ''">
    <main>
      <div class="search-box">
        <input type="text" name="" id="" class="search-bar" placeholder="Search..."
          v-model="query" @keyup.enter="fetchWeather"/>
      </div>
      
      <transition name="view">
        <div class="weather-wrap" v-if="show">
        <div class="location-box">
          <div class="location">{{ weather.city.name }}, {{ weather.city.country }}</div>
          <div class="date">{{ today }}</div>
        </div>
        <div class="weater-box">
          <div class="temp">{{ Math.round(forecastdaysmaxdegs[0]) }}°c</div>
          <div class="weather">{{ forecastdaysmaxforecast[0] }}</div>
        </div>
        <div class="weather-forecast" v-for="forecastday in forecast" v-bind:key="forecastday.id">
          <div class="forecast" >{{ forecastday }} </div>
        </div>
      </div>
      </transition>
    </main>
  </div>
</template>

<script>
export default {
  data(){
    return {
      api_key: '5dc38e4a11491147bd65c298d2e19d2a',
      url_base: 'https://api.openweathermap.org/data/2.5/forecast',
      query: '',
      prevQuery: '',
      show: false,
      weather: {},
      forecastdays: [],
      forecastdaysmaxdegs: [],
      forecastdaysmaxforecast: [],
      forecast: [],
      today: ''
    }
  },
  methods: {
    fetchWeather(){
      if (this.query.localeCompare(this.prevQuery) != 0){
        this.show = false;
        this.prevQuery = this.query;
        fetch(`${this.url_base}?q=${this.query}&units=metric&appid=${this.api_key}&cnt=40`)
        .then(res => {
          return res.json();
        })
        .then(this.setResults)
        .then(this.forecastBuilder)
      }
    },
    setResults(results){
      this.weather = results;
      setTimeout(()=>{
        this.show = true;
      }, 300);
      this.forecastdays = [];
      this.forecastdaysmaxdegs = [];
      this.forecastdaysmaxforecast = [];
      this.forecast = [];
      this.today = '';
      this.query = '';
    },
    forecastBuilder(){
      console.log(this.weather.city.name);
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let startDate = new Date();
      for (let i=0; i<5; i++) {
        let step = new Date();
        step.setDate(startDate.getDate()+i);
        this.forecastdays.push(step.getDate());
        if (i==0){
          this.forecastdaysmaxdegs.push(this.weather.list[0].main.temp);
          this.forecastdaysmaxforecast.push(this.weather.list[0].weather[0].main);
          this.today = days[startDate.getDay()] + ' ' + startDate.getDate() + ' ' + months[startDate.getMonth()] + ' ' + startDate.getFullYear();
        } else {
          let daysdegs = [];
          let daysforecast = [];
          let startday = this.forecastdays[i];
          for(let i=0; i<40; i++){
            if(parseInt(this.weather.list[i].dt_txt.substring(8,10)) == startday){
              daysdegs.push(this.weather.list[i].main.temp);
              daysforecast.push(this.weather.list[i].weather[0].main);
            }
          }
          let daymaxdeg = 0;
          let daymaxforecast = "";
          for(let i=0; i<daysdegs.length; i++){
            if(daymaxdeg < daysdegs[i]){
              daymaxdeg = daysdegs[i];
              daymaxforecast = daysforecast[i];
            }
          }
          this.forecastdaysmaxdegs.push(daymaxdeg);
          this.forecastdaysmaxforecast.push(daymaxforecast);
          this.forecast.push(
            days[step.getDay()] + ' ' + this.forecastdays[i] + ' ' + months[step.getMonth()] + ' ' +
            ' - Up to ' + Math.round(this.forecastdaysmaxdegs[i]) + '°c and ' + this.forecastdaysmaxforecast[i]
          )
        }
      }
      console.log(this.forecast);
    }
  }
}
</script>

<style scoped>
  @import './css/style.css';
</style>