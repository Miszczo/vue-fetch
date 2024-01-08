<script setup>
import { ref, defineEmits, watch, onMounted, nextTick } from 'vue';

const props = defineProps({
    modelValue: Number,
    totalPages: Number
}),
    emit = defineEmits(),
    currentPage = ref(props.modelValue?.currentPage || 1);

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

function updateModel() {
    emit('update:modelValue', currentPage);
}

// @TODO
function setPaginatorNumbers() {
    console.log(props.totalPages)
}

onMounted(() => {
    nextTick(()=> {
        setPaginatorNumbers()
    })
})

</script>

<template>
    {{ totalPages }}
    <button @click="previousPage()">Previous page</button>
    <button @click="nextPage()">Next page</button>
    <button v-for="item in 10"></button>
</template>