<template>
  <div class="weather" :class="weatherClass">
    <div class="container">
      <div class="card-weather-form">
        <input type="text" class="weather-form-input" placeholder="Введите город" v-model="searchQuery" @keyup.enter="weatherSearch"/>
        <button class="weather-form-btn" @click="weatherSearch">Найти</button>
      </div>
      <div class="weather-info">
        <div class="weather-info-text">
          <p class="card">{{location}}</p>
          <p class="card">{{temperature}}</p>
          <p class="card">{{description}}</p>
        </div>
      </div>

    </div>
    <div class="weather-background">
      <div>
        <img class="weather-bg__img bg" src="./assets/images/background.jpg" alt="App background">
        <img class="weather-bg__img overcast" src="./assets/images/overcast.png" alt="App overcast">
        <img class="weather-bg__img partly-cloudy" src="./assets/images/partly-cloudy.png" alt="Partly Cloudy">
        <img class="weather-bg__img sunny" src="./assets/images/sunny.png" alt="Sunny">
      </div>
    </div>
    <div class="card weather-load" v-if="loading">Загрузка...</div>

    <div class="weather-info" v-show="!hasError && location && temperature !== 0 && description">

      <div class="card" v-if="hasError">Возникла ошибка</div>

      <div class="weather-info__text">
        ...
      </div>
    </div>
  </div>
</template>

<script>
  export default {
  data() {
  return {
  location: '',
  temperature: 0,
  description: '',
  searchQuery: '',
  loading: false,
  hasError: false,
};
},

  computed: {
  weatherClass() {
  if (this.description.includes("Sunny")) {
  return 'sunny';
}
  else if (this.description.includes("Overcast")) {
  return 'overcast';
}
  else if (this.description.includes("Partly Cloudy")) {
  return 'partly-cloudy';
}}},

  methods: {
    weatherSearch() {
      this.loading = true;
      this.hasError = false;
      fetch(`https://api.weatherapi.com/v1/current.json?key=3096e8b6661a410aa12165652250307&q=${this.searchQuery}`)
          .then(response => response.json())
          .then(data => {
            this.loading = false;
            this.location = data.location.name;
            this.temperature = data.temperature;
            this.temperature = data.current.temp_c;
            this.description = data.current.condition.text;
            this.resetSearchQuery();
          })
          .catch(error => {
            this.loading = false;
            this.hasError = true;
            console.error(error);
          })
    },
    resetSearchQuery() {
      this.searchQuery = '';
    },
  },
};
</script>