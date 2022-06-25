<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp>16?'warm':''">
      <div class="main">
        <input type="text" class="search" placeholder="Search" v-model="query" @keypress="fetchweather">
        <div 
       v-if="typeof weather.main != 'undefined'"
        > 
         
        <h2 class="title_city">{{weather.name}}, {{weather.sys.country}}</h2>
        <h5 class="title_date">{{dateBulder()}}</h5>
        <div class="weather-box">
            <div class="degree">
                  {{ Math.round(weather.main.temp)}}°C
            </div>
            
        </div>
        <h1 class="weather">{{weather.weather[0].main}}</h1>
        </div>
        <div  v-if="weather.cod === '404'">
          <br>
         <h4 class="title_date"> {{weather.message}}</h4>
        </div>
        
      </div>
  </div>
</template>
<script>
export default {
  name:"app",
  data() {
    return {
      api_key:"2df8edd0755890aca71a76f7e22d685f",
      url_base:"https://api.openweathermap.org/data/2.5/",
      query:"",
      weather:{}
    }
  },
  methods:{
    fetchweather(e){

        if(e.key=="Enter"){
          
          fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res=>{
            return res.json();
          }).then(this.setResults);
        }
    },
  setResults(results){
    this.weather=results
  },
    dateBulder(){
      let d = new Date();
      let months = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'];
       let days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];

       let day=days[d.getDay()];
       let date=d.getDate();
       let month=months[d.getMonth()];
       let year=d.getFullYear();


      return `${day} ${date} ${month} ${year}`;

    }
  }
}
</script>
<style lang="scss">
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.search{
  width: 80%;
  margin-left:10% ;
  margin-top: 20px;
  height: 40px;
  border-radius:0 15px;
  padding-left: 10px;
}
#app.warm{
   background-image: url('./assets/warm-bg.jpg');
  
}
#app{
  background-image: url('./assets/cold-bg.jpg');
  background-size:cover ;
  background-position: bottom;
  height: 100vh;
  transition: 0.4s;
  font-family:sans-serif;
  .main{
  height: 100vh;
  padding: 20px;
  background-image: linear-gradient(to bottom,rgba(0,0,0,0.25),rgba(0,0,0,0.05));


        .title_city,.title_date{
          color: #fff;
          text-align: center;
          margin-top: 30px;
        }
         .title_date{
          color: rgba(255, 255, 255, 0.664)(255, 255, 255, 0.664);
          text-align: center;
          margin-top: 10px;
        }

  .weather-box{
      width: 100%;
      display: flex;
      justify-content: center;
      justify-items: center;

       .degree{
          
          background: rgba(255, 255, 255, 0.575);
          margin-top: 20px;
          padding: 25px 36px;
          font-size: 100px;
          border-radius:10%;
          box-shadow:5px 10px rgba(0,0,0,0.20);
          color: #fff;
          text-shadow: 5px 5px rgba(0,0,0,0.40);

        }
      }

      .weather{text-align: center;
          margin-top: 20px;
          color: #fff;
          text-shadow: 5px 5px rgba(0,0,0,0.20);

      }
       
    }
}


</style>
