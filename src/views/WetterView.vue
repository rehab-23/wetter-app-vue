<template>
  <div class="wetter-app">

    <br><br><br>
    <h1>Renés Wetter-App</h1>
    <br>

    <p style="text-align: center;">{{ currentDate }}</p>

    <div class="input">
      <input type="text" placeholder="Ort eingeben" class="input_text" v-model="city">
      <input type="submit" value="senden" class="submit" @click="loadWeather">
    </div>

    <!-- Ausgabe der Wetterdaten -->
    <div class="container" v-if="weather">
      <div class="card">
        <h1 class="name">{{ weather.name }}</h1>
        <p class="temp">{{ (weather.main.temp - 273.15).toFixed(1) }} °C</p>
        <p class="clouds">{{ weather.weather[0].main }}</p>
        <p class="desc">{{ weather.weather[0].description }}</p>
      </div>
    </div>

  </div>
</template>

<script>
export default {
  data() {
    return {
      city: "",
      weather: null,
      currentDate: new Date().toLocaleDateString("de-DE"),
      apiKey: "DEIN_API_KEY"
    };
  },

  methods: {
    async loadWeather() {
      if (!this.city) {
        alert("Bitte einen Ort eingeben");
        return;
      }

      try {
        const url = `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=${this.apiKey}`;
        const response = await fetch(url);
        const data = await response.json();

        if (data.cod !== 200) {
          alert("!ERROR – Fehlerhafte Eingabe");
          return;
        }

        this.weather = data;
        this.city = "";

      } catch (error) {
        alert("Netzwerkfehler oder ungültige Eingabe");
      }
    }
  }
};
</script>

<style scoped>
@import "@/assets/style.css";
</style>