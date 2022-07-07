<script setup lang="ts">
import { ref } from 'vue'

const totalPage = ref<number>(9)

</script>

<template>
  <div class="flex gap-2 justify-center">
    <!-- first button -->
    <button v-if="totalPage > 0">1</button>

    <!-- rest buttons  -->
    <div v-for="x in totalPage" :key="x">
      <button v-if="x > 1 && x < totalPage">{{ x }}</button>
    </div>

    <!-- last button -->
    <button>{{ totalPage }}</button>
  </div>

  <!-- input container -->
  <div class="mt-4">
    <!-- total page -->
    <legend>
      <label  class="mr-2" for="totalPage">total page</label>
      <input class="border border-slate-200" type="number" v-model="totalPage">
    </legend>
  </div>
</template>