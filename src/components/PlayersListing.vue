<script setup lang="ts">
import { ref, onMounted } from 'vue';
import Player from './Player.vue';
import Paginator from './Paginator.vue'

/**
 * refs
 */

interface PlayerDataType {
    id: number;
    first_name: string;
    last_name: string
}

const responseData = ref<Array<PlayerDataType>>([]),
    apiUrl = ref('https://www.balldontlie.io/api/v1/players'),
    totalPages = ref<number>(10),
    currentPage = ref<number>(1),
    searchText = ref(''),
    queryParams = ref({
        per_page: 5,
        page: 1,
    });
/**
 * methods
 */
async function getData(callback: () => any) {
    try {
        const url = callback();
        const response = await fetch(url.toString());

        if (!response.ok) {
            throw new Error(`HTTP error! Stats: ${response.status}`)
        }

        const data = await response.json();
        responseData.value = [...data.data];

    } catch (error) {
        console.log(error)
    }
}

function createQuery() {
    const urlWithParams = new URL(apiUrl.value);
    Object.entries(queryParams.value).forEach(([key, value]) => {
        urlWithParams.searchParams.set(key, String(value));
    })
    return urlWithParams;
}

function createSearchQuery() {
    const urlWithParams = new URL(apiUrl.value);
    urlWithParams.searchParams.set("search", searchText.value);

    return urlWithParams;
}

function handleModelUpdate(newModelValue: number) {
    currentPage.value = newModelValue;
    queryParams.value.page = currentPage.value;
    getData(createQuery);
}

function searchPlayer() {
    getData(createSearchQuery);
}

/**
 * hooks
 */

onMounted(() => {
    getData(createQuery)
})

</script>

<template>
    <div>
        <input v-model="searchText" type="text">
        <button @click="searchPlayer">Search</button>
    </div>
    <div class="players-wrapper">
        <Player v-for="playerData in responseData" :key="playerData.id" :playerData="playerData" />
    </div>
    <Paginator v-model="currentPage" :total-pages="totalPages" @update:modelValue="handleModelUpdate" />
</template>

<style scoped>
.players-wrapper {
    max-width: 800px;
}
</style>