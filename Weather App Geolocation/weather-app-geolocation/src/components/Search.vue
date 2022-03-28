<template>
    <div>
    <div class="search-box">
    <input 
        type="text"
        class="search-bar"
        placeholder="Search..."
        v-model="cities"
        @keypress="getWeather"
        />
        <button class="detection" 
                @click="getlocation">
        <img src="../assets/target.png"/>
        </button>
    </div>
    </div>
</template>

<script>
import axios from "axios";

export default {
    data () {
        return {
    cities: '',
    url: 'http://api.openweathermap.org/data/2.5/weather',
    url_seven: 'http://api.openweathermap.org/data/2.5/forecast/daily',
    KEY: 'ff22e1da27d3000c755237d8be6183e9',
    KEY_2:'7550def670411d2e1842ecc9e2118b9c',
    user_location:false,


        }
    },
    methods: {
    getWeather(e) {
        if(e.key == "Enter"){
            axios
                .get(this.url, { params: { appid:this.KEY, q:this.cities, units:"metric"}})
                .then(response => { this.$emit('weather', response.data); })

            axios.get(this.url_seven, { params: { appid:'20571ab45c74dc2a1897b60c5b8047a1', q:this.cities, units:"metric", cnt:7}})
            .then(res => { this.$emit('weather_seven', res.data)})
        }
    },
    
    getlocation() {
    this.user_location=true;
        if(navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(data => { 
            this.getGeolocation(data) });
        }
        else {
                console.log("Geolocation is not supported by this browser.");
            }
    },
    
    getGeolocation(data) {
    axios(`https://api.openweathermap.org/data/2.5/weather?lat=${data.coords.latitude}&lon=${data.coords.longitude}&units=metric&appid=ff22e1da27d3000c755237d8be6183e9`)
    .then((res) => {
            this.$emit('user_value', res.data)
                })
        },
    beforeMount() {
    this.getlocation();
    },
    }
}
</script>

<style>

.search-box {
width: 75%;
margin-bottom: 50px;
margin-right: 30px;
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
border-radius: 0px 16px 0px 16px;
transition: 0.4s;
}
.search-box .search-bar:focus {
box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
background-color: rgba(255, 255, 255, 0.75);
border-radius: 16px 0px 16px 0px;
}

.search-box .detection {  
    position: absolute;
    right: 370px;
    top: 82px;
    height: 55px;
    width: 55px;
    border-radius: 55%;
}


</style>