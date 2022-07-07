<script setup lang="ts">
import { computed, ref } from 'vue'

const totalPage = ref<number>(150)
const currentPage = ref<number>(6)
const boundary = ref<number>(2)
const sibling = ref<number>(1)
const disablePrevious = ref<boolean>(true)
const disableNext = ref<boolean>(true)
const disableFirst = ref<boolean>(true)
const disableLast = ref<boolean>(true)
const disableAll = ref<boolean>(true)

const pagination = ref<Array<number>>([1,2])

const result = computed(() => { 
  if (
    boundary.value > 0 
    && boundary.value < totalPage.value 
    // && totalPage.value > (currentPage.value + 2 * boundary.value + 2 * sibling.value)
  ) {
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

      if (currentPage.value >= (totalPage.value - sibling.value)) {
      } else {
        pagination.value.push(0)
        let lastValue: Array<number> = []
        for (let z = (totalPage.value); z > (totalPage.value - boundary.value); z--) {
          if (z <= totalPage.value) {
            lastValue.push(z)
          }
        }

        lastValue = lastValue.sort((a, b) => a - b)

        pagination.value = [...pagination.value, ...lastValue]
      }
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
  if (currentPage.value > 0) {
    currentPage.value = currentPage.value - 1
  }
}

function toggleNext() {
  disableNext.value = !disableNext.value
}

function nextHandle() {
  if (currentPage.value < totalPage.value) {
    currentPage.value = currentPage.value + 1
  }
}

function toggleFirst() {
  disableFirst.value = !disableFirst.value
}

function firstHandle() {
  currentPage.value = currentPage.value 
}

function toggleLast() {
  disableLast.value = !disableLast.value
}

function lastHandle() {
  currentPage.value = totalPage.value
}

function toggleDisable() {
  disableAll.value = !disableAll.value
}

function toCurrentPage(numberPage:number) {
  if (currentPage.value < totalPage.value) {
    currentPage.value = numberPage
  }
}
</script>

<template>
  <div class="border-2 border-dashed border-slate-400 p-4 mx-32 rounded">
    <div class="mt-[-30px] mb-5">
      <span class="bg-white px-3 text-slate-400">Pagination</span>
    </div>
    <div class="flex gap-2 justify-center text-slate-500">
      <button class="hover:cursor-pointer" @click="firstHandle" :disabled="disableFirst">First</button>
      <button class="cursor-pointer" @click="previousHandle" :disabled="disablePrevious">Previous</button>
      <div v-for="x in (result)" :key="x">
        <button 
          @click="toCurrentPage(x)"
          :class="[currentPage === x ? 'bg-blue-100' : '', 'cursor-pointer rounded-full p-2 mx-2']" 
        >{{ x === 0 ? '..' : x }}</button>
      </div>
      <button class="cursor-pointer" @click="nextHandle" :disabled="disableNext">Next</button>
      <button class="cursor-pointer" @click="lastHandle" :disabled="disableLast">Last</button>
    </div>
    <!-- input container -->

    <div class="mt-6 flex justify-center gap-10 text-slate-500">
      <div>
        <!-- total page -->
        <legend class="flex justify-between mb-3">
          <label class="mr-2" for="totalPage">Total page</label>
          <input class="border border-slate-200 w-12 text-center" type="number" v-model="totalPage">
        </legend>

        <!-- current page -->
        <legend class="flex justify-between mb-3">
          <label class="mr-2" for="currentPage">Current page</label>
          <input class="border border-slate-200 w-12 text-center" type="number" v-model="currentPage">
        </legend>

        <!-- boundary -->
        <legend class="flex justify-between mb-3">
          <label class="mr-2" for="boundary">Boundary</label>
          <input class="border border-slate-200 w-12 text-center" type="number" v-model="boundary">
        </legend>

        <!-- sibling -->
        <legend class="flex justify-between mb-3">
          <label class="mr-2" for="sibling">Sibling</label>
          <input class="border border-slate-200 w-12 text-center" type="number" v-model="sibling">
        </legend>
      </div>
      <div>
        <!-- previous -->
        <legend class="flex justify-between mb-3 items-center">
          <label class="mr-2" for="sibling">Previous</label>
          <input type="checkbox" name="previous" @change="togglePrevious">
        </legend>
        
        <!-- next -->
        <legend class="flex justify-between mb-3 items-center">
          <label class="mr-2" for="sibling">Next</label>
          <input type="checkbox" name="previous" @change="toggleNext">
        </legend>

        <!-- first -->
        <legend class="flex justify-between mb-3 items-center">
          <label class="mr-2" for="sibling">First</label>
          <input type="checkbox" name="previous" @change="toggleFirst">
        </legend>

        <!-- last -->
        <legend class="flex justify-between mb-3 items-center">
          <label class="mr-2" for="sibling">Last</label>
          <input type="checkbox" name="previous" @change="toggleLast">
        </legend>

        <!-- disable -->
        <legend class="flex justify-between mb-3 items-center">
          <label class="mr-2" for="sibling">Disable</label>
          <input type="checkbox" name="previous" @change="toggleDisable">
        </legend>
      </div>

    </div>
  </div>
</template>