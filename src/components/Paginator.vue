<script setup>
import { ref, watch } from 'vue';

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
    currentPageRef = ref(props.modelValue?.currentPage || 1),
    totalPagesRef = ref(0),
    paginationNumbersRef = ref([]);

/**
 * watch
 */

watch(() => props.totalPages, (newTotalPages) => {
    totalPagesRef.value = newTotalPages;
    generatePaginationNumbersArray(newTotalPages);
    createPaginationNumberButtons();
});


/**
* methods
*/

function nextPage() {
    if (currentPageRef.value < props.totalPages) {
        currentPageRef.value++;
        updateModel();
    }
}

function previousPage() {
    if (currentPageRef.value > 1) {
        currentPageRef.value--;
        updateModel();
    }
}

function goToPage(number) {
    currentPageRef.value = number;
    updateModel();
}

function generatePaginationNumbersArray(n) {
    paginationNumbersRef.value = Array.from({ length: n }, (_, index) => index + 1);
}

function createPaginationNumberButtons() {
    paginationNumbersRef.value = paginationNumbersRef.value.slice(0, 9);
}

function checkAndGenerateNewPagination() {
    const newStart = Math.max(1, currentPageRef.value - 4);
    const newEnd = Math.min(newStart + 8, totalPagesRef.value);
    paginationNumbersRef.value = Array.from({ length: 9 }, (_, index) => newStart + index).slice(0, newEnd - newStart + 1);
}

function updateModel() {
    checkAndGenerateNewPagination()
    emit('update:modelValue', currentPageRef);
}

</script>

<template>
    <button @click="previousPage()">Previous page</button>
    <button v-if="currentPageRef > 5" @click="goToPage(1)">First page</button>
    <button v-for="item in paginationNumbersRef" @click="goToPage(item)" :class="{ 'is-active': item == currentPageRef }"
        :key="item">
        {{ item }}
    </button>
    <button @click="nextPage()">Next page</button>
    <button @click="goToPage(totalPagesRef)">Last page</button>
</template>

<style lang="scss">
button {
    &.is-active {
        background-color: red;
        pointer-events: none;
    }
}
</style>