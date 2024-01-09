<script setup>
import { ref, watch, onMounted, nextTick, onUpdated } from 'vue';

const props = defineProps({
    modelValue: Number,
    totalPages: Number
}),
    emit = defineEmits(),
    currentPage = ref(props.modelValue?.currentPage || 1),
    paginationNumbers = ref([]);

function nextPage() {
    if (currentPage.value < props.totalPages) {
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

function goToPage(number) {
    currentPage.value = number;
    updateModel();
}

function updateModel() {
    emit('update:modelValue', currentPage);
}

// @TODO - napisać funkcjonalnośc paginacji któa wylistuje liczby od 2 do 10, ale  im dalej będziemy przewijać to te liczby będą się zmieniać
function setPaginatorNumbers(number) {
    for (let i = 2; i < number; i++) {
        paginationNumbers.value.push(i)
    }
}

onMounted(()=> {
    setPaginatorNumbers(11)
})

onUpdated(() => {
    // Reset values when the page is changed from outside of this component
})

</script>

<template>
    <button @click="previousPage()">Previous page</button>
    <button v-for="item in paginationNumbers" @click="goToPage(item)" :key="item">{{ item }}</button>
    <button @click="goToPage(totalPages)">Last page</button>
    <button @click="nextPage()">Next page</button>
</template>