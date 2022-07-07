<script setup lang="ts">
import { computed, ref } from 'vue'

const totalPage = ref<number>(10)
const currentPage = ref<number>(5)
const boundary = ref<number>(1)
const sibling = ref<number>(1)

const pagination = ref<Array<number>>([1,2])

const result = computed(() => { 
  // if (totalPage.value < (sibling.value * 2)) {
    // pagination.value = Array.from({length: totalPage.value}, (_, i) => i + 1) 
  // }

  if (boundary.value > 0 && boundary.value < totalPage.value) {
    pagination.value = Array.from({length: boundary.value}, (_, i) => i + 1) 
    pagination.value.push(0)
    
    if (sibling.value > 0) {
      // rest value
      for (let y = (currentPage.value - sibling.value); y < (currentPage.value + sibling.value + 1); y++) {
        pagination.value.push(y)
      }

      // pagination.value = [...pagination.value, ]

      pagination.value.push(0)
      let lastValue: Array<number> = []
      for (let z = (totalPage.value); z > (totalPage.value - boundary.value); z--) {
        lastValue.push(z)
      }

      lastValue = lastValue.sort((a, b) => a - b)

      pagination.value = [...pagination.value, ...lastValue]
    }


    // pagination.value.push(totalPage.value)

    // pagination.value.splice(boundary.value, 0, 0)

    // pagination.value.splice(pagination.value.length - boundary.value, 0, 0)
  } else {
    pagination.value = Array.from({length: totalPage.value}, (_, i) => i + 1) 
  }

  return pagination.value 
})
</script>

<template>
  <div class="flex gap-2 justify-center">
    <div v-for="x in (result)" :key="x">
      <button 
        :class="[currentPage === x ? 'bg-blue-100' : '']" 
      >{{ x === 0 ? '..' : x }}</button>
    </div>
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