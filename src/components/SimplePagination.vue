<script setup lang="ts">
import { ref } from 'vue'

const totalPage = ref<number>(9)
const currentPage = ref<number>(9)
const boundary = ref<number>(9)
const sibling = ref<number>(9)

</script>

<template>
  <!-- <div v-if="totalPage > 2" class="flex gap-2 justify-center"> -->
  <div class="flex gap-2 justify-center">
    <!-- first button -->
    <button :class="[currentPage === 1 ? 'bg-blue-100' : '']" v-if="totalPage > 0">1</button>

    <!-- boundary for first button -->
    <div v-for="x in (boundary + 1)" :key="x">
      <button :class="[currentPage === x ? 'bg-blue-100' : '']" v-if="x > 1 && x < totalPage">{{ x }}</button>
    </div>

    <!-- three dot after first boundary -->
    <p v-if="totalPage > 3 && boundary > 0 && ((boundary + 1) > currentPage - sibling)">...</p>

    <div v-if="currentPage > (boundary + 1) && currentPage < (totalPage - boundary)" class="flex gap-2">
      <!-- siblings before current page  -->
      <div class="flex flex-row-reverse gap-2">
        <div v-for="x in sibling" :key="x">
          <button>{{ currentPage - x }}</button>
        </div>
      </div>

      <!-- current page -->
      <button class="bg-blue-100">{{ currentPage }}</button>

      <!-- siblings after current page  -->
      <div v-for="x in sibling" :key="x">
        <button>{{ currentPage + x }}</button>
      </div>
    </div>

    <!-- three dots after last boundary -->
    <p v-if="totalPage > 3 && boundary > 0 && currentPage > (boundary + sibling)">...</p>

    <!-- boundary for last button -->
    <div v-for="x in (boundary + 1)" :key="x">
      <button :class="[currentPage === totalPage - (boundary - (x - 2)) ? 'bg-blue-100' : '']" v-if="x > 1 && x < totalPage">{{ totalPage - (boundary - (x - 2)) }}</button>
    </div>

    <!-- last button -->
    <button  v-if="totalPage > 1" :class="[currentPage === totalPage ? 'bg-blue-100' : '']">{{ totalPage }}</button>
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