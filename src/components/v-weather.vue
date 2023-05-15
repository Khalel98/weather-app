<template>
    <div class="app-wrap" :style="cssProps">
    <header>
      <input type="text" autocomplete="off" class="search-box" v-model="inputCity" placeholder="Search for a city or country..."  v-on:keyup.enter="eventSearchInput"/>

    </header>
    <main>
      <section class="location">
        <div class="city">{{ this.city }}</div>
        <div class="date">{{ this.date }}</div>
      </section>

      <div class="current">
        <div class="temp">{{ this.temp }}</div>
        <div class="weather">{{ this.weather }}</div>
        <div class="hi-low">{{ this.highLow }}</div>
        <div class="hi-low">{{ this.icon }}</div>
      </div>
    </main>

    <p style="color:azure">{{ term }}</p>
  </div>
</template> 

<script>
export default {
   data(){
      return{
        inputCity:'',
        url: `https://api.openweathermap.org/data/2.5/weather?q=`,
        key: `9b81d3e4265e2d1a5b8b73f1591ec139`,  
        city: '',
        date:'',
        temp: '',
        weather:'',
        highLow:'',
        icon:'',
        cssProps: {
          backgroundImage: ``
        },
        term:''
      }
   },
   methods:{
      getData(city) {
         fetch(`${this.url}${city}&appid=${this.key}`)
            .then(res => res.json())
            .then(data => {
               this.showData(data);
              // console.log(data);
               this.inputCity = ''
            })
            .catch(res => {
               alert("There is no such country (404)");
               this.inputCity = ''

            });
      },
      showData(data) {
        this.city = data.name + ", " + data.sys.country
        this.date = this.showDate();
        this.temp = `${Math.floor(data.main.temp - 273.15)}  °c`
        this.weather = data.weather[0].main
        this.highLow = `${Math.floor(data.main.temp_max - 273.15)} °c / ${Math.floor(data.main.temp_min - 273.15)} °c`
        this.icon = data.weather[0].icon

        this.checkWeather(data)

      },
      
      checkWeather(data){
        let icon = data.weather[0].icon
        //clear sky
        if(icon == '01d' || icon == '01n'){
          this.cssProps.backgroundImage = `url(${require('@/assets/clouds.gif')})`
        }
        //few clouds
        else if(icon == '02d' || icon == '02n'){
          this.cssProps.backgroundImage = `url(${require('@/assets/clouds.gif')})`
        }
        //scattered clouds
        else if(icon == '03d' || icon == '03n'){
          this.cssProps.backgroundImage = `url(${require('@/assets/clouds.gif')})`
        }
        //broken clouds
        else if(icon == '04d' || icon == '04n'){
          this.cssProps.backgroundImage = `url(${require('@/assets/clouds.gif')})`
        }
        //shower rain
        else if(icon == '09d' || icon == '09n'){
          this.cssProps.backgroundImage = `url(${require('@/assets/rain.gif')})`
        }
        //rain
        else if(icon == '10d' || icon == '10n'){
          this.cssProps.backgroundImage = `url(${require('@/assets/rain.gif')})`
        }
        //thunderstorm
        else if(icon == '11d' || icon == '11n'){
          this.cssProps.backgroundImage = `url(${require('@/assets/thunder.gif')})`
        }
        //snow
        else if(icon == '13d' || icon == '13n'){
          this.cssProps.backgroundImage = `url(${require('@/assets/snow.gif')})`
        }
        //mist
        else if(icon == '50d' || icon == '50n'){
          this.cssProps.backgroundImage = `url(${require('@/assets/smog.gif')})`
        }
        
      },
       showDate() { 
        let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      
        let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      
        let now = new Date();
      
        let day = days[now.getDay()];
        let month = months[now.getMonth()];
        let year = now.getFullYear();
        let date = now.getDate();
      
        return `${day} ${date} ${month} ${year}`;
      },

      eventSearchInput() {
         this.getData(this.inputCity);
      },
      getGeolocationInformation() {
        fetch('http://ip-api.com/json/')
            .then(res => res.json())
            .then(data => {

              this.getData(data.city)
            })
      }
   },
   mounted(){
    this.getGeolocationInformation();

   }
}
</script>


<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  
  .app-wrap {
    font-family: 'montserrat', sans-serif;
    /*background-image: url('https://raw.githubusercontent.com/hosseinghafouri/weather-app-real-API/main/content/images/bg.jpg');*/
    background-size: cover;
    background-position: top center;
  }
  
  .app-wrap {
    display: flex;
    flex-direction: column;
    min-height: 100vh;
    background-color: linear-gradient(to bottom, rgba(0, 0, 0, 0.3), rgba(0, 0, 0, 0.6));
  }
  
  header {
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 50px 15px 15px;
  }
  
  header input {
    width: 100%;
    max-width: 300px;
    padding: 10px 15px;
    border: none;
    outline: none;
    background-color: rgba(255, 255, 255, 0.3);
    border-radius: 16px 0px 16px 0px;
    border-bottom: 3px solid #DF8E00;
    
    color: #313131;
    font-size: 20px;
    font-weight: 300;
    transition: 0.2s ease-out;
  }
  
  header input:focus {
    background-color: rgba(255, 255, 255, 0.6);
  }
  
  main {
    flex: 1 1 100%;
    padding: 25px 25px 50px;
    display: flex;
    flex-direction: column;
    align-items: center;
    text-align: center;
  }
  
  .location .city {
    color: #FFF;
    font-size: 32px;
    font-weight: 500;
    margin-bottom: 5px;
  }
  
  .location .date {
    color: #FFF;
    font-size: 16px;
  }
  
  .current .temp {
    color: #FFF;
    font-size: 102px;
    font-weight: 900;
    margin: 30px 0px;
    text-shadow: 2px 10px rgba(0, 0, 0, 0.6);
  }
  
  .current .temp span {
    font-weight: 500;
  }
  
  .current .weather {
    color: #FFF;
    font-size: 32px;
    font-weight: 700;
    font-style: italic;
    margin-bottom: 15px;
    text-shadow: 0px 3px rgba(0, 0, 0, 0.4);
  }
  
  .current .hi-low {
    color: #FFF;
    font-size: 24px;
    font-weight: 500;
    text-shadow: 0px 4px rgba(0, 0, 0, 0.4);
  }   
</style>



