<script setup>
import { reactive, ref, onMounted, watch  } from 'vue';
import Player from './Player.vue';

/**
 * refs
 */

const apiUrl = ref('https://www.balldontlie.io/api/v1/players');
const responseData = ref();

/**
 * Reactives
 */
// Parametry zapytania (np. limit wyników)
const queryParams = reactive({
    per_page: 10,  // Liczba wyników na stronę
    page: 0,       // Numer strony
});

// Konstruowanie URL z parametrami
const urlWithParams = new URL(apiUrl.value);
const searchParams = new URLSearchParams(queryParams);
urlWithParams.search = searchParams;

/**
 * reactives
 */
const movies = reactive({})

/**
 * methods
 */
await function getData(url) {
    try{
        const response = await fetch(url);
        
        if(!response.ok) {
            throw new Error(`HTTP error! Stats: ${response.status}`)
        }
    }
    fetch(url)
        .then(response => response.json())
        .then(response => {
            responseData.value = response;

        }
        )
        .catch(error => console.log("Error: ", error))
}

getData(urlWithParams)


onMounted(() => {

})
watch(() => responseData.value, (newData, oldData) => {
    console.log(newData); // Wywoła się po zmianie responseData.value
});

</script>

<template>
    <!-- <div v-for="playerData in responseData.value">
            <span>{{ playerData }}</span>
        </div> -->
    ============
    <pre>{{ responseData }}</pre>
</template>

<style scoped></style>