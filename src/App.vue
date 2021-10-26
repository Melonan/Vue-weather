<template>
  <div id="app" :class="typeof weather.main!='undefined' && weather.main.temp>16?'warm':''">
    <main>
      
      <div class="search-box">
        <input 
          type="text" 
          class="search-bar" 
          placeholder="Search..."
          v-model="query"
          @keypress="fetchWeather"
        >
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        
        <div class="location-box">
          <div class="location">{{weather.name}}, {{weather.sys.country}}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          
          <div class="temp">{{Math.round(weather.main.temp)}}℃</div>
          <div class="weather">{{weather.weather[0].main}}</div>
          <div class="weather-description">{{weather.weather[0].description}}</div>
          
        </div>   

      </div>





    </main>
  </div>
</template>

<script>

export default {
  name: 'App',
  data(){
    return {
      api_key:'0e7dd38e74e7e9b0df869da17ae29ed6',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query:'',
      weather:{}
    }
  },
  methods:{
    fetchWeather(e){
      if(e.key == "Enter")
      {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
        .then(res => {
          return res.json();
        }).then(this.setResults);
      }
    },
    setResults(results)
    {
      this.weather = results;
    },
    dateBuilder(){
      let d = new Date();
      let months = ["Jan", "Feb" ,"Mar", "Apr", "May", 
      "Jun", "Jul", "Aug","Sep", "Ocu","Nov", "Dec"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      console.log(d);
      console.log(`${day} ${date} ${month} ${year}`);

      return `${day} ${date} ${month} ${year}`;
    }
  }

}
</script>

<style>
*{
  margin:0;
  padding: 0;
  box-sizing: border-box;
}

body{
  font-family: 'montserrat', sans-serif;
}

#app{
  background-image: url('./assets/cold-bg.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s; 

}

#app .warm{
  background-image: url('./assets/warm-bg.jpg');
}

main{
  min-height: 100vh;
  padding: 25px;

 /* 线性渐变函数  to bottom 指从顶到底部进行渐变 ,  从上到下慢慢不透明*/
  background-image: linear-gradient(to bottom, rgba(0,0,0,0.25),rgba(0,0,0,0.75));
}



.search-box{
  width: 100%;
  margin-bottom: 30px;
}

.search-box .search-bar {


/*
display 属性的值 
  none	此元素不会被显示。
  block	此元素将显示为块级元素，此元素前后会带有换行符。
  inline	默认。此元素会被显示为内联元素，元素前后没有换行符。 
*/
  display: block;
  width: 100%;
  padding: 15px;
/* 这里的color 实际上改变的是search bar中的字体颜色 */
  color: #313131;
  font-size: 20px;

  appearance: none;
  border: none;
  outline: none;
  background: none;




  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255,255,255,0.15);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

/* 级联选择器 */
/*  注意 : search-bar 一定要写到search-box的后面 */
.search-box .search-bar:focus {
 box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
 background-color: rgba(255, 255, 255, 0.849);
 border-radius: 16px 0px 16px 0px;

}

.location-box .location{
  color: #FFF;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date{
  color: #FFF;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}


.weather-box{
  text-align: center;
}

.weather-box .temp{
  display: inline-block;
  padding: 10px 25px;
  color:#FFF;
  font-size: 102px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0 , 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  /* 30px  上下  0px: 左右 */
  margin: 30px 0px;



/*
Shadow:
h-shadow	必需的。水平阴影的位置。允许负值
v-shadow	必需的。垂直阴影的位置。允许负值
blur	可选。模糊距离
spread	可选。阴影的大小
color	可选。阴影的颜色。在CSS颜色值寻找颜色值的完整列表
 */

  box-shadow: 3px 6px 6px  rgba(0, 0, 0, 0.25);

}

.weather-box .weather{
  color:#FFF;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
}

.weather-box .weather-description{
  color:rgba(120, 199, 212, 0.808);
  font-size: 15px;
  font-weight: 200;
  font-style: oblique 20deg;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.507);
}


</style>
