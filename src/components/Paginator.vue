<script setup>
import { ref, watch, onMounted, nextTick, onUpdated } from 'vue';

/**
* props
*/

const props = defineProps({
    modelValue: Number,
    totalPages: Number
});

/**
 * refs
 */

const emit = defineEmits(),
    currentPage = ref(props.modelValue?.currentPage || 1),
    paginationStartNumber = ref(1),
    paginationNumbers = ref([]);

/**
* methods
*/

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


function updatePaginationNumbers() {
    if (currentPage.value == paginationNumbers.value[paginationNumbers.value.length - 1]) {
        setPaginatorNumbers(9, paginationNumbers.value[paginationNumbers.value.length - 1] + 1);
    } else if (currentPage.value == paginationNumbers.value[0]) {
        const newStartingNumber = Math.max(paginationStartNumber.value, paginationNumbers.value[0] - 9);
        setPaginatorNumbers(9, newStartingNumber);
    }
}


function setPaginatorNumbers(numberOfButtons = 9, startingNumber = paginationStartNumber.value) {
    paginationNumbers.value = [];
    for (let i = 0; i < numberOfButtons; i++) {
        paginationNumbers.value.push(startingNumber + i);
    }
}

function updateModel() {
    updatePaginationNumbers();
    emit('update:modelValue', currentPage);
}
/**
 * hooks
 */

onMounted(() => {
    setPaginatorNumbers(9)
})

</script>

<template>
    <button @click="previousPage()">Previous page</button>
    <button v-for="item in paginationNumbers" @click="goToPage(item)" :key="item">{{ item }}</button>
    <button @click="nextPage()">Next page</button>
    <button @click="goToPage(totalPages)">Last page</button>
</template>