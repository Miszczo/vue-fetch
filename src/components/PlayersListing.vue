<script setup>
import { ref, onMounted, watch } from 'vue';
import Player from './Player.vue';
import Paginator from './Paginator.vue'

/**
 * refs
 */

const responseData = ref([]),
    apiUrl = ref('https://www.balldontlie.io/api/v1/players'),
    totalPages = ref(0),
    currentPage = ref(1),
    queryParams = ref({
        per_page: 10,
        page: 1,
    });

/**
 * methods
 */
async function getData() {
    try {
        const urlWithParams = createUrl();
        const response = await fetch(urlWithParams);

        if (!response.ok) {
            throw new Error(`HTTP error! Stats: ${response.status}`)
        }

        const data = await response.json();
        responseData.value = [...data.data];
        totalPages.value = data.meta.total_pages;

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

function handleModelUpdate(newModelValue) {
    currentPage.value = newModelValue;
    queryParams.value.page = currentPage.value;
    getData();
}

/**
 * hooks
 */

onMounted(() => {
    getData()
})

</script>

<template>
    <Paginator v-model="currentPage" :total-pages="totalPages" @update:modelValue="handleModelUpdate" />
    <div class="players-wrapper">
        <Player v-for="playerData in responseData" :key="playerData.id" :playerData="playerData" />
    </div>
</template>

<style scoped>
.players-wrapper {
    max-width: 800px;
}
</style>