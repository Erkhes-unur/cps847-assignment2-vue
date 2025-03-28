<script setup>
import { ref, watchEffect } from "vue";

const temperature = ref(null);
const error = ref(null);

watchEffect(async () => {
    try {
        const API_KEY = "8e898e601ec889181e57184e8f8c4273";
        const url = "https://api.openweathermap.org/data/2.5/weather?units=metric&q=Belleville,Ontario,CA&appid=" + API_KEY;
        const response = await fetch(url);
        if (!response.ok) throw new Error("No weather data...");
        const data = await response.json();
        console.log(data);
        temperature.value = data.main.temp;
    } catch (err) {
        error.value = err.message;
    }
});
</script>

<template>
    <div class="weather-info">
        <h2>Belleville, Ontario Weather</h2>
        <div v-if="error" class="error">Error: {{ error }}</div>
        <div v-else-if="temperature !== null" class="temp">Current Temperature: {{ temperature }}°C</div>
        <div v-else class="loading">Getting data...</div>
    </div>
</template>

<style scoped>
.weather-info {
    font-family: Arial, sans-serif;
    margin: 1em;
}

.error {
    color: red;
    font-size: 1.2em;
}

.loading {
    color: gray;
    font-size: 1.2em;
}

.temp {
    font-size: 1.2em;
}
</style>
