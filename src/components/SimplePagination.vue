<script setup lang="ts">
import { ref } from 'vue'

const totalPage = ref<number>(9)
const currentPage = ref<number>(9)
const boundary = ref<number>(9)
const sibling = ref<number>(9)

</script>

<template>
  <div class="flex gap-2 justify-center">
    <!-- first button -->
    <button :class="[currentPage === 1 ? 'bg-blue-100' : '']" v-if="totalPage > 0">1</button>

    <!-- rest buttons  -->
    <div v-for="x in totalPage" :key="x">
      <button :class="[currentPage === x ? 'bg-blue-100' : '']" v-if="x > 1 && x < totalPage">{{ x }}</button>
    </div>

    <!-- last button -->
    <button :class="[currentPage === totalPage ? 'bg-blue-100' : '']">{{ totalPage }}</button>
  </div>

  <!-- input container -->
  <div class="mt-4">
    <!-- total page -->
    <legend>
      <label class="mr-2" for="totalPage">Total page</label>
      <input class="border border-slate-200" type="number" v-model="totalPage">
    </legend>

    <!-- current page -->
    <legend>
      <label class="mr-2" for="currentPage">Current page</label>
      <input class="border border-slate-200" type="number" v-model="currentPage">
    </legend>

    <!-- current page -->
    <legend>
      <label class="mr-2" for="boundary">Boundary</label>
      <input class="border border-slate-200" type="number" v-model="boundary">
    </legend>
    <!-- current page -->
    <legend>
      <label class="mr-2" for="sibling">Sibling</label>
      <input class="border border-slate-200" type="number" v-model="sibling">
    </legend>
  </div>
</template>