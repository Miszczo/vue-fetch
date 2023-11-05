<script setup>
import { reactive, ref, onMounted, watch } from 'vue';
import Player from './Player.vue';

/**
 * refs
 */

const responseData = ref();
const apiUrl = ref('https://www.balldontlie.io/api/v1/players');

const queryParams = ref({
    per_page: 10,
    page: 1,
});


/**
 * methods
 */
async function getData(url) {
    console.log(url)
    try {
        const response = await fetch(url);

        if (!response.ok) {
            throw new Error(`HTTP error! Stats: ${response.status}`)
        }

        const data = await response.json();
        responseData.value = data.data;
        console.log(responseData.value)
    } catch (error) {
        console.log(error)
    }
}

function createUrl() {
    const urlWithParams = new URL(apiUrl.value);
    const searchParams = new URLSearchParams(queryParams.value);
    urlWithParams.search = searchParams;

    return urlWithParams;
}

function appendData() {
    queryParams.value.page += 1;
    getData(createUrl())
}

onMounted(() => {
    getData(createUrl())
})

watch(() => responseData.value, (newData, oldData) => {
    responseData.value = newData
});

</script>

<template>
    <div class="players-wrapper" v-for="playerData in responseData">
        <Player :playerData="playerData" />
    </div>
    <button @click="appendData()">append</button>
</template>

<style scoped>
.players-wrapper {
    max-width: 800px;
}
</style>