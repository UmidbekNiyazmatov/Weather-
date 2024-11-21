

<script setup>
import { ref } from "vue";

const appKey = "c50c704a23b5b932c25e982e9fa7bef6";
const query = ref("");
const urlBase = "https://api.openweathermap.org/data/2.5/";
const weather = ref(null);

const fetchData = async () => {
  if (query.value) {
    try {
      const response = await fetch(
        `${urlBase}weather?q=${query.value}&units=metric&appid=${appKey}`
      );

      if (!response.ok) {
        throw new Error("Failed to fetch weather data");
      }
      const data = await response.json();
      weather.value = data; 
    } catch (error) {
      console.log(error);
    } finally {
      query.value = ""; 
    }
  }
};
</script>

<template>
  <div>
    <div class="div" >
    
      <h1>Weather App <span>
      <img src="/public/icons8-weather.svg" alt="">
    </span></h1>
      
      <div>
        <input
          type="text"
          placeholder="Enter a location"
          v-model="query"
          @keyup.enter="fetchData"
        />
        <button @click="fetchData">Check</button>
      </div>

      <div v-if="weather">
        <h2>
          {{ weather.name }},
          <span>{{ weather.sys.country }}</span>
        </h2>
        <div >
          <h3>{{ Math.floor(weather.main.temp) }} °C</h3>
          <p>{{ weather.weather[0].main }}</p>
          
        </div>
      </div>
    </div>
  </div>
</template>



<style scoped>
*{

  
  margin: 0;
  padding: 0;

}

body {
  margin: 0;
  padding: 0;
  font-family: Arial, sans-serif;
  background: linear-gradient(to right, #06b6d4, #3b82f6);
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  
 
}

.div {
  text-align: center;
  background: linear-gradient(to right, #06b6d4, #3b82f6);
  height: 98vh ;
  color: white;
 
}

h1 {
  text-align: center;
  font-size: 2.5rem;
  margin-bottom: 20px;
  
}



input[type="text"] {
  padding: 10px;
  font-size: 1rem;
  border: none;
  border-radius: 5px;
  width: 300px;
}

button {
  padding: 10px 20px;
  font-size: 1rem;
  color: white;
  background-color: #1e90ff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  margin-left: 10px;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #005ecb;
}

h2 {
  margin-top: 20px;
  font-size: 2rem;
}

h3 {
  font-size: 2.5rem;
  margin: 10px 0;
}

p {
  font-size: 1.2rem;
  margin: 10px 0;
}

img {
  transform: translateY(20px);

}
</style>

