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
        // totalPages.value = data.meta.total_pages;

    } catch (error) {
        console.log(error)
    }
}

function createUrl() {
    const urlWithParams = new URL(apiUrl.value);
    Object.entries(queryParams.value).forEach(([key, value]) => {
        urlWithParams.searchParams.set(key, String(value));
    })

    return urlWithParams;
}

function handleModelUpdate(newModelValue: number) {
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