<template>
    <div id="app">
      <div class="mainn">
          <div class="search-box">
                <input class="search-bar" type="text" placeholder="Search..." v-model="data.city" @keyup.enter="getApi()">
                    <div v-if="data.weather">
                    <div class="header">
                        <h1>{{data.weather.name}}</h1>
                        <h3>{{ new Date().toLocaleString('vi-VN') }}</h3>
                    </div>
                        <div class="temp">
                        <h2>{{data.weather.main.temp}}&deg;</h2>
                        </div>
                        <div class="state">
                        <h3>{{ data.weather.weather[0].main}}</h3>
                        </div>
                    </div>
                    <div v-if="data.erroMsg" class="erroMessage">
                        <h3>{{ data.erroMsg }}</h3>
                    </div>
            </div>
        </div>
    </div>
    </template>
    
    <script>
    import axios from "axios"
    export default {
      name: 'weather',
      data(){
        return{
          data:{
            city:'',
            weather: '',
            erroMsg:''
          }
        }
      },
      mounted: async function(){
          this.getApi()
      },
    methods:{
        async getApi(){
        if(this.data.city===''){
          this.data.city = 'Ho Chi Minh'
        }
                try {
                    const getWeather = await axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.data.city}&appid=e36c683cfcadeee7c83cf50dd7212247&units=metric`)
                    this.data.weather = getWeather.data
                    this.data.erroMsg =''
                    this.data.city = ''
                } catch (error) {
                    this.data.weather = '';
                    this.data.erroMsg = 'Thành phố không tồn tại'
                }
            }
        }

    }
</script>
    
<style scoped>
* {
margin: 0 auto;
padding: 0;
box-sizing: border-box;
}

body {
font-family: 'Montserrat', sans-serif;
background-color: #f0f8ff;
display: flex;
justify-content: center;
align-items: center;
height: 100vh;
}

#app {
text-align: center;
padding: 20px;
background-color: #ffffff;
border-radius: 8px;
box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.2);
max-width: 400px;
width: 100%;
}

.search-box {
margin-bottom: 20px;
}

.search-bar {
width: 100%;
padding: 10px;
font-size: 16px;
border: 1px solid #ccc;
border-radius: 4px;
outline: none;
}

.header h1 {
font-size: 24px;
color: #333;
margin-bottom: 5px;
}

.header h3 {
font-size: 14px;
color: #666;
margin-bottom: 20px;
}

.temp h2 {
font-size: 48px;
font-weight: bold;
color: #ff4500;
}

.state h3 {
font-size: 20px;
color: #555;
}


</style>