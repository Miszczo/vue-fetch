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

// @TODO
function checkPaginationButtons() {
    console.log(currentPage.value)
    if(currentPage.value > (paginationNumbers.value.length * 0.9)) {
        console.log("higher")
    }
    // console.log(paginationNumbers.value.length * 0.75)
    // console.log(paginationNumbers.value.indexOf(currentPage.value))
    // console.log(paginationNumbers.value.includes(currentPage.value))
}

function updateModel() {
    checkPaginationButtons()
    emit('update:modelValue', currentPage);
}

// @TODO - napisać funkcjonalnośc paginacji któa wylistuje liczby od 2 do 10, ale  im dalej będziemy przewijać to te liczby będą się zmieniać
function setPaginatorNumbers(numerOfButtons) {
    for (let i = 0; i < numerOfButtons; i++) {
        paginationNumbers.value.push(i + 2)
    }
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
    <button @click="goToPage(totalPages)">Last page</button>
    <button @click="nextPage()">Next page</button>
</template>