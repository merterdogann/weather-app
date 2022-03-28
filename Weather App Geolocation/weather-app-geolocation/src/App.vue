<template>
  <div id="app">
    <h1 class="title">Weather App</h1>
    <main>
        <Search  @user_value="getUser_Value" @weather="getWeather_Value"  @weather_seven="getWeather_Seven"/>

        <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
          <div class="location-box">
            <div class="location">
              {{ weather.name }}, {{ weather.sys.country }}
            </div>
            <div class="date"> {{ dateBuilder() }}</div>
          </div>
          <div class="weather-box">
            <div class="temp"> {{ Math.round(weather.main.temp) }}°c </div>
            <br /> 
            <div class="humidity"> Humidity: {{ weather.main.humidity }} %</div>
            <div class="wind"> Wind:{{ weather.wind.speed}} m/s</div>
            <br />
            <div class="weather">{{ weather.weather[0].description }} </div>
            <br />
              <div class="icon">
              <img :src="`${weather_icon}${weather.weather[0].icon}${'@2x.png'}`"/>

            <div class="seven-conditions">
          <span v-for="data in weather_seven.list" :key="data.id">
            <p> {{data.day}} </p>
            <div class="weather-user-icon"> <img :src="`${weather_icon}${data.weather[0].icon}${'@2x.png'}`"/> </div>
            <p>{{ data.weather[0].main }}</p>
          
          <p>{{ parseInt(data.temp.day) }}° / {{ parseInt(data.temp.night) }}°</p>
          </span> 
          </div>



        <div class="user-weather"   v-show="user_location=!user_location"  v-if="typeof user_value.main != 'undefined'">
          <div class="user-location-box">
            <div class="user-location">
              {{ user_value.name }}, {{ user_value.sys.country }}
            </div>
            <div class="user-date"> {{ dateBuilder() }}</div>
          </div>
          <div class="user-weather-box">
            <div class="user-temp"> {{ Math.round(user_value.main.temp) }}°c </div>
            <br /> 
            <div class="user-humidity"> Humidity: {{ user_value.main.humidity }} %</div>
            <div class="user-wind"> Wind:{{ user_value.wind.speed}} m/s</div>
            <br />
            <div class="user-weather">{{ user_value.weather[0].description }} </div>
            <div class="user-icon"> <img :src="`${weather_icon}${user_value.weather[0].icon}${'@2x.png'}`"/> </div>
              </div>
            </div>
            </div>
          </div>
          </div>
        </main>
        </div>
</template>

<script>
import Search from './components/Search.vue'
export default {
  components: { Search },
  data() {
    return {
    weather_icon: 'http://openweathermap.org/img/wn/',
    weather: '',
    weather_seven: '',
    user_value:'',
    user_location:false,
    days: ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'],
    daysIndex: [],

    }
    },

    methods: {
      getUser_Value(data){
        this.user_value = data;
      },
      getWeather_Value(data){
        this.weather = data;
      },
      getWeather_Seven(data){
        this.weather_seven = data;
        console.log(this.weather_seven);
      },
    
  dateBuilder() {
    let d = new Date();
    let months = ["January", "February", "March", "April", "May", "June", "July", 
    "August", "September", "October", "November", "December"];
    let days = this.days;
    let day = days[d.getDay()];
    let date = d.getDate();
    let month = months[d.getMonth()];
    let year = d.getFullYear();

    return `${day} ${date} ${month} ${year}`;
  },
    
    },
    mounted() {

    var currentDate = new Date();
    var nextWeek = new Date(currentDate.getTime() + 7 * 24 * 60 * 60 * 1000);
    var days = []
    while (currentDate <= nextWeek) {
      days.push(new Date(currentDate).getDay());
      currentDate.setDate(currentDate.getDate() + 1);
    }


    this.daysIndex = days.slice(1);

  },
    watch:{
  weather_seven:function(){
  for (let i = 0; i < this.daysIndex.length; i++) {
    this.weather_seven.list[i].day = this.days[this.daysIndex[i]]

    }
  
  }
}

}
</script>

<style>
.title {
  font-family: Arial, Helvetica, sans-serif;
  font-size: 50px;
  text-align: center;
}

* {
margin: 0;
padding: 0;
box-sizing: border-box;
}

body {
font-family: 'montserrat', sans-serif;
}

#app {
  
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;

}

main {
min-height: 100vh;
padding: 25px;
background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}

.location-box .location, .user-location-box  .user-location {
color: #fff;
font-size: 50px;
font-weight: 500;
text-align: center;
text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.date, .user-date {
color: rgb(36, 17, 17);
font-style: normal;
padding-top: 10px;
font-size: 25px;
font-weight: 300;
font-style: italic;
text-align: center;
}
.weather-box, .user-weather-box {
text-align: center;
}
.weather-box .temp, .user-weather-box .user-temp {
display: inline-block;
padding: 10px 25px;
color: #fff;
font-size: 92px;
font-weight: 700;
text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
background-color: rgba(255, 255, 255, 0.25);
border-radius: 16px;
margin: 30px 0px;
box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .humidity, .user-weather-box .user-humidity{
display:inline-block;
padding: 10px 25px;
color: #fff;
font-size: 35px;
font-weight: 600;
font-style: italic;
text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
border-radius: 15px;
margin: 30px 0px;    
}

.weather-box .weather, .user-weather-box .user-weather {
color: lightgray;
text-transform: capitalize;
font-size: 40px;
font-weight: 700;
font-style: italic;
text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .wind, .user-weather-box .user-wind {
color: lightgray;
text-transform: capitalize;
font-size: 40px;
font-weight: 700;
font-style:oblique;
text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.seven-conditions {
  display: flex;
  flex-direction:row;
  align-items: center;
  justify-content:space-around;
  font-size: 14px;
  
}


</style>
