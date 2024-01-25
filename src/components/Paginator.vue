<script setup lang="ts">
import { onMounted, ref, watch } from 'vue';

/**
* props
*/

interface PaginatorProps {
  modelValue: number;
  totalPages: number;
}

const props = defineProps<PaginatorProps>();

/**
 * refs
 */

const emit = defineEmits(),
  currentPageRef = ref(props.modelValue || 1),
  totalPagesRef = ref(0),
  paginationNumbersRef = ref<number[]>([]);

/**
 * watch
 */

// watch(() => props.totalPages, (newTotalPages) => {
//   totalPagesRef.value = newTotalPages;
//   // generatePaginationNumbersArray(newTotalPages);
//   // createPaginationNumberButtons();
// });


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

// function goToPage(number: number) {
//   currentPageRef.value = number;
//   updateModel();
// }

// function generatePaginationNumbersArray(number: number) {
//   paginationNumbersRef.value = Array.from({ length: number }, (_, index) => index + 1);
// }

// function createPaginationNumberButtons() {
//   paginationNumbersRef.value = paginationNumbersRef.value.slice(0, 9);
// }

// function checkAndGenerateNewPagination() {
//   const newStart = Math.max(1, currentPageRef.value - 4);
//   const newEnd = Math.min(newStart + 8, totalPagesRef.value);
//   paginationNumbersRef.value = Array.from({ length: 9 }, (_, index) => newStart + index).slice(0, newEnd - newStart + 1);
// }

function updateModel() {
  // checkAndGenerateNewPagination()
  emit('update:modelValue', currentPageRef);
}

// onMounted(() => {
//   generatePaginationNumbersArray(10);
// })
</script>

<template>
  <button @click="previousPage()">Previous page</button>
  <!-- <button v-if="currentPageRef > 5" @click="goToPage(1)">First page</button> -->
  <!-- <button v-for="item in paginationNumbersRef" @click="goToPage(item)" :class="{ 'is-active': item == currentPageRef }"
    :key="item">
    {{ item }}
  </button> -->
  <button @click="nextPage()">Next page</button>
  <!-- <button @click="goToPage(totalPagesRef)">Last page</button> -->
</template>

<style lang="scss">
button {
  &.is-active {
    background-color: red;
    pointer-events: none;
  }
}
</style>
