<script setup lang="ts">
import { computed, ref } from 'vue'

const totalPage = ref<number>(10)
const currentPage = ref<number>(5)
const boundary = ref<number>(1)
const sibling = ref<number>(1)
const disablePrevious = ref<boolean>(true)

const pagination = ref<Array<number>>([1,2])

const result = computed(() => { 
  if (boundary.value > 0 && boundary.value < totalPage.value && totalPage.value > (currentPage.value + 2 * boundary.value + 2 * sibling.value)) {
    pagination.value = Array.from({length: boundary.value}, (_, i) => i + 1) 
    pagination.value.push(0)
    
    if (sibling.value > 0) {
      // rest value
      for (let y = (currentPage.value - sibling.value); y < (currentPage.value + sibling.value + 1); y++) {
        if (!pagination.value.includes(y)) {
          pagination.value.push(y)
        } else {
          pagination.value.splice(boundary.value, 1)
        }
      }

      pagination.value.push(0)
      let lastValue: Array<number> = []
      for (let z = (totalPage.value); z > (totalPage.value - boundary.value); z--) {
        lastValue.push(z)
      }

      lastValue = lastValue.sort((a, b) => a - b)

      pagination.value = [...pagination.value, ...lastValue]
    }
  } else {
    pagination.value = Array.from({length: totalPage.value}, (_, i) => i + 1) 
  }

  return pagination.value 
})

function togglePrevious() {
  disablePrevious.value = !disablePrevious.value
}

function previousHandle() {
  currentPage.value = currentPage.value - 1
}

</script>

<template>
  <div class="flex gap-2 justify-center">
    <button @click="previousHandle" :disabled="disablePrevious">Previous</button>
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

    <!-- boundary -->
    <legend>
      <label class="mr-2" for="boundary">Boundary</label>
      <input class="border border-slate-200" type="number" v-model="boundary">
    </legend>

    <!-- sibling -->
    <legend>
      <label class="mr-2" for="sibling">Sibling</label>
      <input class="border border-slate-200" type="number" v-model="sibling">
    </legend>

    <!-- sibling -->
    <legend>
      <label class="mr-2" for="sibling">Previous</label>
      <input type="checkbox" name="previous" @change="togglePrevious">
    </legend>
    
  </div>
</template>