<script setup lang="ts">
import { computed, ref } from 'vue'
import { ChevronRightIcon, ChevronDoubleRightIcon, ChevronLeftIcon, ChevronDoubleLeftIcon } from '@heroicons/vue/solid'

const totalPage = ref<number>(10)
const currentPage = ref<number>(6)
const boundary = ref<number>(2)
const sibling = ref<number>(1)
const disablePrevious = ref<boolean>(true)
const disableNext = ref<boolean>(true)
const disableFirst = ref<boolean>(true)
const disableLast = ref<boolean>(false)
const disableAll = ref<boolean>(false)

const pagination = ref<Array<number>>([1,2])

const result = computed(() => { 
  if (
    boundary.value > 0 
    && boundary.value < totalPage.value 
  ) {
    pagination.value = Array.from({length: boundary.value}, (_, i) => i + 1) 
    pagination.value.push(0)
    
    if (sibling.value > 0 && sibling.value < totalPage.value) {
      // rest value
      for (let y = (currentPage.value - sibling.value); y < (currentPage.value + sibling.value + 1); y++) {
        if (!pagination.value.includes(y)) {
          if (y <= totalPage.value && y > 0) {
            pagination.value.push(y)
          }
        } else {
          pagination.value.splice(boundary.value, 1)
        }
      }

      let lastValue: Array<number> = []
      if (currentPage.value < (totalPage.value - boundary.value)) {
        pagination.value.push(0)

        for (let z = totalPage.value + 1 - boundary.value; z <= totalPage.value; z++) {
          lastValue.push(z)
        }
      } 

      if (lastValue.length > 0) {
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
  if (disableAll.value) {
    disablePrevious.value = true
  } else {
    disablePrevious.value = !disablePrevious.value
  }
}

function previousHandle() {
  if (currentPage.value > 0) {
    currentPage.value = currentPage.value - 1
  } else {
    currentPage.value = currentPage.value
  }
}

function toggleNext() {
  if (disableAll.value) {
    disableNext.value = true
  } else {
    disableNext.value = !disableNext.value
  }
}

function nextHandle() {
  if (currentPage.value < totalPage.value) {
    currentPage.value = currentPage.value + 1
  } else {
    currentPage.value = currentPage.value
  }
}

function toggleFirst() {
  if (disableAll.value) {
    disableFirst.value = true
  } else {
    disableFirst.value = !disableFirst.value
  }
}

function firstHandle() {
  currentPage.value = 1
}

function toggleLast() {
  if (disableAll.value) {
    disableLast.value = true
  } else {
    disableLast.value = !disableLast.value
  }
}

function lastHandle() {
  currentPage.value = totalPage.value
}

function toggleDisable() {
  disableAll.value = !disableAll.value
}

function toCurrentPage(numberPage:number) {
  if (numberPage <= totalPage.value) {
    currentPage.value = numberPage
  } else {
    currentPage.value =  currentPage.value
  }
}
</script>

<template>
  <div class="border-2 border-dashed border-slate-400 p-4 mx-32 rounded">
    <div class="mt-[-30px] mb-5">
      <span class="bg-white px-3 text-slate-400">Pagination</span>
    </div>
    <div class="flex gap-2 justify-center text-slate-500">
      <button class="hover:cursor-pointer" @click="firstHandle" :disabled="disableFirst"><ChevronDoubleLeftIcon class="text-slate-400 w-5" /></button>
      <button class="cursor-pointer" @click="previousHandle" :disabled="disablePrevious"><ChevronLeftIcon class="text-slate-400 w-5" /></button>

      <div v-for="(x, key) in result" :key="key">
        <button 
          @click="toCurrentPage(x)"
          :class="[
            currentPage === x ? 'bg-blue-100' : ''
            , 'cursor-pointer rounded-full p-2 mx-2'
            , disableAll === true ? 'text-slate-300' : 'text-slate-500'
          ]" 
          :disabled="disableAll"
        >{{ x === 0 ? '..' : x }}</button>
      </div>
      <button class="cursor-pointer" @click="nextHandle" :disabled="disableNext"><ChevronRightIcon class="text-slate-400 w-5" /></button>
      <button class="cursor-pointer" @click="lastHandle" :disabled="disableLast"><ChevronDoubleRightIcon class="text-slate-400 w-4" /></button>
    </div>
    <!-- input container -->

    <div class="mt-6 flex justify-center gap-10 text-slate-500">
      <div>
        <!-- total page -->
        <legend class="flex justify-between mb-3 items-center">
          <label :class="['mr-2', disableAll ? 'text-slate-300' : '']" for="next">Total Page</label>
          <input class="border border-slate-200 w-12 text-center" type="number" v-model="totalPage" :disabled="disableAll">
        </legend>

        <!-- current page -->
        <legend class="flex justify-between mb-3 items-center">
          <label :class="['mr-2', disableAll ? 'text-slate-300' : '']" for="currentPage">Current Page</label>
          <input class="border border-slate-200 w-12 text-center" type="number" v-model="currentPage" :disabled="disableAll">
        </legend>

        <!-- boundary -->
        <legend class="flex justify-between mb-3 items-center">
          <label :class="['mr-2', disableAll ? 'text-slate-300' : '']" for="next">Boundary</label>
          <input class="border border-slate-200 w-12 text-center" type="number" v-model="boundary" :disabled="disableAll">
        </legend>

        <!-- sibling -->
        <legend class="flex justify-between mb-3 items-center">
          <label :class="['mr-2', disableAll ? 'text-slate-300' : '']" for="sibling">Sibling</label>
          <input class="border border-slate-200 w-12 text-center" type="number" v-model="sibling" :disabled="disableAll">
        </legend>
      </div>
      <div>
        <!-- previous -->
        <legend class="flex justify-between mb-3 items-center">
          <label :class="['mr-2', disableAll ? 'text-slate-300' : '']" for="previous">Previous</label>
          <input type="checkbox" name="previous" @change="togglePrevious" :disabled="disableAll">
        </legend>
        
        <!-- next -->
        <legend class="flex justify-between mb-3 items-center">
          <label :class="['mr-2', disableAll ? 'text-slate-300' : '']" for="next">Next</label>
          <input type="checkbox" name="next" @change="toggleNext" :disabled="disableAll">
        </legend>

        <!-- first -->
        <legend class="flex justify-between mb-3 items-center">
          <label :class="['mr-2', disableAll ? 'text-slate-300' : '']" for="first">First</label>
          <input type="checkbox" name="first" @change="toggleFirst" :disabled="disableAll">
        </legend>

        <!-- last -->
        <legend class="flex justify-between mb-3 items-center">
          <label :class="['mr-2', disableAll ? 'text-slate-300' : '']" for="last">Last</label>
          <input type="checkbox" name="last" @change="toggleLast" :disabled="disableAll">
        </legend>

        <!-- disable -->
        <legend class="flex justify-between mb-3 items-center">
          <label class="mr-2" for="sibling">Disable All</label>
          <input type="checkbox" name="previous" @change="toggleDisable">
        </legend>
      </div>
    </div>
  </div>
</template>