<script setup>
import { ref, defineEmits, watch } from 'vue';

const props = defineProps(['modelValue']),
    emit = defineEmits(),
    currentPage = ref(props.modelValue?.currentPage || 1),
    totalPages = ref(props.modelValue?.totalPages || 0);

function nextPage() {
    if (currentPage.value < totalPages.value) {
        currentPage.value++;
        updateModel();
    }
}

function previousPage() {
    if (currentPage.value > 1) {
        currentPage.value--;
        updateModel();
    }
}

function updateModel() {
    emit('update:modelValue', { currentPage: currentPage.value, totalPages: totalPages.value });
}

watch(() => props.modelValue, (newModelValue) => {
    currentPage.value = newModelValue.currentPage;
    totalPages.value = newModelValue.totalPages;
});
</script>

<template>
    <button @click="previousPage()">Previous page</button>
    <button @click="nextPage()">Next page</button>

    <button v-for="item in 20"></button>
</template>